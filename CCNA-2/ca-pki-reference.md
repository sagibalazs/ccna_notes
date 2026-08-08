# Certificate Authorities & PKI — Technical Reference

Scope: the whole CA topic, from first principles down to the field level, framed for
your OPNsense device CA now and your FreeIPA org PKI later.

---

## 1. The core problem a CA solves

Asymmetric (public-key) cryptography gives you two primitives:

- **Encryption to a public key** — anyone can encrypt to your public key; only your private key decrypts.
- **Digital signatures** — you sign with your private key; anyone can verify with your public key.

What it does **not** give you: any guarantee about **whose** public key you are holding.
If an attacker hands you *their* public key while claiming to be your bank, the math still
works perfectly — you'd encrypt secrets straight to the attacker.

A **Certificate Authority** solves precisely this identity-binding gap. It is a trusted
third party that **binds an identity to a public key** by signing a data structure that
says, in effect: *"I, the CA, vouch that this public key belongs to this identity, and here
is my signature to prove I said it."* That signed statement is a **certificate**.

**Trust is transitive.** If a device already trusts the CA's public key, it will
automatically trust anything the CA has signed — without ever having met those end entities
before. This is the whole point: it lets trust scale to millions of parties who never
directly exchanged keys.

---

## 2. What a certificate actually is (X.509)

A certificate is **not** a key. It is a **signed data structure** (X.509 v3 format,
DER-encoded ASN.1, often wrapped in Base64 as PEM). Two logical parts:

### 2a. The TBS ("To Be Signed") body

| Field | Meaning |
|---|---|
| **Version** | Almost always v3 |
| **Serial Number** | Unique per issuing CA; used for revocation lookups |
| **Signature Algorithm** | e.g. `sha256WithRSAEncryption`, `ecdsa-with-SHA384` (key type + digest) |
| **Issuer** | DN of the CA that signed this cert |
| **Validity** | notBefore / notAfter dates |
| **Subject** | DN of the identity this cert is *about* |
| **Subject Public Key Info** | The public key being bound, + its algorithm |
| **Extensions** | v3 extensions (SAN, keyUsage, basicConstraints, etc. — see §8) |

### 2b. The signature

The CA computes a **hash of the entire TBS body** using a digest algorithm, then **signs
that hash with the CA's private key**. That signature is appended.

**Verification (what every TLS client does):**
1. Recompute the hash over the TBS body yourself.
2. Verify the signature against that hash using the **CA's public key** (from the CA cert).
3. If it validates → the cert has not been tampered with **and** was genuinely signed by that CA.

This is why **key type** and **digest algorithm** both matter: they are the two halves of
step 1+2, and either being weak undermines the binding.

---

## 3. The signing / verifying mechanism, concretely

```
SIGNING (done once, by the CA):
   TBS body ──hash(SHA-x)──▶ digest ──sign(CA private key)──▶ signature

VERIFYING (done by every client, every connection):
   TBS body ──hash(SHA-x)──▶ digest_A
   signature ──verify(CA public key)──▶ digest_B
   digest_A == digest_B ?  → valid : invalid
```

Two independent failure surfaces:
- **Weak digest** → attacker crafts a *different* TBS body with the *same* hash (collision) → forged cert the CA never intended. This is why **SHA-1 is dead**.
- **Weak/leaked signing key** → attacker signs *anything* as the CA. This is why the **CA private key is the crown jewel** and why offline roots exist (§7).

---

## 4. Key types (the "Subject Public Key" algorithm)

### RSA
- Security rests on the hardness of factoring large integers.
- Sizes: 2048 (floor today), 3072, 4096.
- **Pros:** universal compatibility — every browser, every legacy VPN client, every switch.
- **Cons:** large keys, slower signing/handshakes, key size grows fast for higher security.

### ECDSA (Elliptic Curve DSA)
- Security rests on the elliptic-curve discrete-log problem.
- Curves: P-256 (secp256r1), P-384 (secp384r1), P-521.
- **Pros:** far smaller keys for equivalent strength, faster handshakes, lower CPU — the direction enterprise and TLS are moving.
- **Cons:** a rare ancient client may not support it (irrelevant for the internal clients *you* control).

### EdDSA (Ed25519)
- Newest, fast, misuse-resistant, deterministic signatures.
- **Cons:** not always exposed in appliance UIs (OPNsense's CA screen typically offers RSA + ECDSA, not Ed25519). Great where supported.

### Security-equivalence (NIST SP 800-57)

| Symmetric strength | RSA modulus | Elliptic curve | Typical digest |
|---|---|---|---|
| 112-bit | RSA 2048 | P-224 | SHA-224/256 |
| **128-bit** | **RSA 3072** | **P-256** | SHA-256 |
| **192-bit** | RSA 7680 | **P-384** | **SHA-384** |
| 256-bit | RSA 15360 | P-521 | SHA-512 |

Read the rows across: **P-384 (192-bit) is stronger than RSA 4096** and dramatically
smaller/faster than the RSA 7680 that would match it. That's the modern-root argument.

---

## 5. Digest / hash algorithms (the signature hash)

The digest is what gets signed. Its collision resistance is a hard security boundary.

| Algorithm | Status |
|---|---|
| MD5 | Broken — never use |
| SHA-1 | Broken (collisions demonstrated) — never use for signing |
| SHA-256 | Standard baseline, pairs with 128-bit keys |
| SHA-384 | Pairs with P-384 / 192-bit keys |
| SHA-512 | Pairs with the strongest keys |

**Match digest to key strength.** A P-384 key signed with SHA-256 is a mismatch (192-bit
key, 128-bit digest); use SHA-384. The `signatureAlgorithm` field encodes the pair, e.g.
`ecdsa-with-SHA384` or `sha384WithRSAEncryption`.

---

## 6. The Distinguished Name (DN) and identity fields

The **Subject** and **Issuer** are each a DN — an ordered set of attributes:

| Attr | Name | Notes |
|---|---|---|
| CN | Common Name | Historically the hostname; **now largely ignored by browsers for TLS name-matching** |
| O | Organization | Cosmetic for internal use, but fill it for realism |
| OU | Organizational Unit | e.g. a department; `OU=1` on your current CA is placeholder junk |
| C | Country | 2-letter (AT) |
| ST | State/Province | e.g. STMK |
| L | Locality | City |
| emailAddress | — | Legacy; avoid embedding real emails |

**Critical modern rule:** for server TLS, the **Subject Alternative Name (SAN)** — *not*
the CN — is authoritative. Chrome/Firefox/etc. have required a matching SAN since ~2017 and
ignore CN entirely for hostname validation. A server cert **must** list every name/IP it
serves under SAN (DNS: and IP: entries). CN is essentially a human-readable label now.

---

## 7. The hierarchy: roots, intermediates, leaves

```
        Root CA           self-signed (Issuer == Subject), CA:TRUE
           │              private key = crown jewel; ideally OFFLINE
           ▼
    Intermediate / Issuing CA    signed by Root, CA:TRUE, online, does day-to-day signing
           │
           ▼
   Leaf / End-Entity certs       CA:FALSE — servers, clients, VPN peers, users
```

- **Root CA** — self-signed; its Issuer equals its Subject. It is the trust anchor you
  install into trust stores. If its key leaks, the *entire* PKI is compromised, so the
  enterprise pattern is to keep the root **offline** (powered off / air-gapped) and only
  power it on to sign intermediates.
- **Intermediate (issuing) CA** — signed by the root, kept online, handles the churn of
  issuing leaf certs. If it's compromised you revoke *it* and re-issue under a new one
  without burning the root.
- **Leaf / end-entity** — the actual server, client, or user cert. `basicConstraints`
  marks it `CA:FALSE` so it can't sign further certs.

A client validates by building a **chain** from the leaf up to a trusted root, verifying
each signature link and each certificate's constraints along the way.

> **Reality check for OPNsense:** its built-in CA is always online — a true offline root
> isn't achievable there. So a **single online root** is the pragmatic choice for that
> device's scope. The proper offline-root + issuing-intermediate story is what **FreeIPA /
> Dogtag** gives you later, and that separation is deliberate.

**Self-signed vs CA-signed leaf:** a self-signed cert *is* its own issuer — no third party
vouches for it, so clients throw warnings unless you manually trust each one. A CA-signed
leaf inherits trust from the CA automatically. That inheritance is the entire value of
running your own CA instead of scattering self-signed certs everywhere.

---

## 8. The v3 extensions that actually control behavior

Extensions are where a cert's *permissions* live. The important ones:

| Extension | Purpose |
|---|---|
| **basicConstraints** | `CA:TRUE`/`CA:FALSE` (+ optional `pathlen`) — can this cert sign other certs? |
| **keyUsage** | Low-level allowed operations: `keyCertSign` + `cRLSign` on a CA; `digitalSignature` + `keyEncipherment` on a server leaf |
| **extendedKeyUsage (EKU)** | High-level intent: `serverAuth` (TLS server), `clientAuth` (TLS/EAP client), `emailProtection`, `codeSigning` |
| **subjectAltName (SAN)** | The authoritative names/IPs a leaf is valid for (DNS:, IP:, email:, URI:) |
| **Subject Key Identifier (SKI)** | Hash of this cert's public key |
| **Authority Key Identifier (AKI)** | Points to the issuer's SKI — lets clients build the chain unambiguously |
| **CRL Distribution Points** | Where to fetch the revocation list |
| **Authority Info Access (AIA)** | OCSP responder URL + issuer-cert URL |

Mismatched extensions are a common failure: e.g. a leaf missing `serverAuth` EKU, or a
"server" cert with no SAN, will be rejected by strict clients even though the signature is valid.

---

## 9. Internal vs External CAs

This is the split you asked about, and it's fundamentally about **who already trusts the
root**.

### External / Public CA
- Examples: Let's Encrypt (free, ACME-automated), DigiCert, Sectigo, Google Trust Services.
- Their roots ship **pre-installed** in every OS and browser trust store.
- **Use for:** anything reachable by devices you *don't* control, or public-facing services.
- **Constraints:**
  - Must prove control of a **publicly resolvable** domain (domain validation).
  - **Cannot** issue for private IPs (192.168.x), or fabricated internal names (`.local`, `.lan`, `.internal`) — those aren't publicly verifiable.
  - Subject to CA/Browser Forum rules: max leaf lifetime is shrinking (398 days now, trending toward ~47 days), certificate transparency logging, rate limits.

### Internal / Private CA
- You run it (OPNsense CA, FreeIPA/Dogtag, step-ca, Microsoft ADCS).
- Trusted by **only** the devices where you've installed its root.
- **Use for:** WebGUI HTTPS, VPN, IPsec, 802.1X/EAP-TLS, internal service mTLS, client-cert auth.
- **Strengths:**
  - Issue for **any** name or IP you like, including internal-only hostnames and private IPs.
  - Full control of lifetimes, key types, extensions.
  - No cost, no rate limits, no public exposure of your internal names in CT logs.
- **The catch:** **trust-store distribution.** Nothing trusts your root until you put it
  there yourself. That's the entire operational burden of a private CA (see §10).

**Rule of thumb:** public CA for anything the public touches; private CA for everything
inside your walls where you control the clients.

---

## 10. Trust stores and how a client validates a chain

A **trust store** is the client's list of root CAs it will believe. There are several,
often independent:
- **OS store** (Windows cert store, macOS Keychain, Linux `/etc/ssl/certs` + `ca-certificates`).
- **Browser store** (Firefox ships its own; Chrome/Edge/Safari mostly use the OS store).
- **Per-application / per-runtime** (Java keystore, Python `certifi`, Node, curl).

**Validation steps a client runs on every handshake:**
1. **Build the chain** — leaf → intermediate(s) → a root in the trust store (using AKI/SKI).
2. **Verify each signature** up the chain.
3. **Check validity dates** at every level.
4. **Check revocation** (CRL/OCSP) if configured.
5. **Check name match** — requested hostname/IP against the leaf's **SAN**.
6. **Check EKU/keyUsage** — is this cert actually allowed to be a TLS server?

Any single failure → the whole connection is rejected/warned.

**Distributing your internal root** is therefore step zero of using a private CA. Options,
roughly in order of scale:
- Manual install into each device's OS/browser store (fine for a handful).
- Config management (**Ansible** pushing the root into `/usr/local/share/ca-certificates` + `update-ca-certificates`) — your path.
- **FreeIPA enrollment** — enrolled hosts automatically trust the IPA CA. This is the big
  operational payoff of FreeIPA vs a bare CA: enrollment *is* trust distribution.
- MDM / Group Policy in corporate environments.

---

## 11. Revocation

Certs are trusted until they expire — but sometimes you must **kill one early** (private
key leaked, device decommissioned, employee left). Two mechanisms:

- **CRL (Certificate Revocation List)** — the CA publishes a signed list of revoked serial
  numbers. Clients download it periodically. Simple, but stale between refreshes and grows
  over time.
- **OCSP (Online Certificate Status Protocol)** — client asks an OCSP responder "is serial
  N still good?" in real time. **OCSP stapling** has the *server* fetch and attach a recent
  signed status to its handshake, removing the client's extra round-trip and the privacy leak.

**Homelab pragmatics:** running CRL/OCSP infrastructure is real overhead. For small internal
PKIs, **short certificate lifetimes** are often a better lever than revocation machinery — a
90-day leaf that you rotate automatically is barely worth revoking. Keep revocation in mind
for the CA certs themselves and for anything long-lived (client/VPN certs).

---

## 12. The lifecycle / workflow

The single most important concept here: **where the private key is born and whether it ever
moves.**

### Path A — CSR flow (private key never leaves the subject) ✅ best practice
```
1. Subject (e.g. a web server) generates its OWN keypair locally.
2. Subject builds a CSR (Certificate Signing Request): its public key + desired DN/SAN,
   self-signed with its own private key to prove possession.
3. CSR is sent to the CA (public info only — private key stays put).
4. CA validates identity, sets extensions, signs → issues the certificate.
5. Cert is returned and installed alongside the key that never moved.
```
The CA **never sees the private key.** This is how public CAs and proper PKI work.

### Path B — CA-generates-everything (convenient, weaker)
```
1. CA generates the keypair AND the cert in one step.
2. You download key + cert together and install on the target.
```
OPNsense's "Create an internal certificate" does this by default. Fast for a homelab, but
the private key has **touched the CA host** and traveled to you — a wider exposure surface.
Fine for low-stakes internal leaf certs; avoid for anything sensitive.

### Full lifecycle
```
generate key → CSR → issue → distribute → monitor → renew (before expiry) → revoke (if needed) → expire
```
Renewal is the recurring operational task; expiry surprises are the #1 cause of "the whole
thing broke at 2am." Automation (ACME internally via step-ca, or IPA's certmonger) exists to
kill that failure mode.

---

## 13. Use cases (mapped to your world)

| Use case | Cert roles | Where in your build |
|---|---|---|
| **WebGUI HTTPS** | server leaf (serverAuth, SAN=mgmt IP+FQDN) signed by device CA | **now** — your immediate task |
| **OpenVPN / WireGuard** | server cert + per-client certs (mutual TLS) | OPNsense VPN later |
| **IPsec** | gateway certs | optional |
| **802.1X / EAP-TLS** | per-device/user **client** certs (clientAuth) | P3 — FreeRADIUS + FreeIPA |
| **Client-cert auth to services** | client leaf presented to a server | app tier |
| **Internal service mTLS** | both ends hold certs, verify each other | k3s / app tier |
| **Code/config signing** | codeSigning EKU | advanced |

---

## 14. How the two CAs in your design fit together

You deliberately have **two separate PKIs**, and that separation is correct:

**OPNsense device CA (`opnsense-web-ca`)**
- Scope: firewall-**local** services only — WebGUI HTTPS, and later OPNsense's own VPN/IPsec certs.
- Lives and dies with the firewall. Small blast radius. Simple.

**FreeIPA / Dogtag org PKI (P3, later)**
- Scope: the **organization** — host certs, service certs, user certs, all tied to Kerberos
  identity, with enrollment doing automatic trust distribution.
- This is your "real" enterprise PKI with proper lifecycle automation.

**Why not one CA for everything?** Blast radius, scope, and lifecycle independence. The
firewall shouldn't depend on the identity platform being up to serve its own admin page
(cold-start/circular dependency — the same foundation-tier principle behind keeping DNS/AAA
out of k3s). A device signing its own local leaf is self-contained and always available.

---

## Glossary / acronyms

| Term | Meaning |
|---|---|
| **PKI** | Public Key Infrastructure — the whole system of CAs, certs, trust, revocation |
| **CA** | Certificate Authority |
| **X.509** | The certificate format standard |
| **DN** | Distinguished Name (Subject / Issuer identity) |
| **CN / O / OU / C / ST / L** | Common Name / Org / Org Unit / Country / State / Locality |
| **CSR** | Certificate Signing Request |
| **SAN** | Subject Alternative Name (authoritative TLS names/IPs) |
| **EKU** | Extended Key Usage (serverAuth, clientAuth, …) |
| **SKI / AKI** | Subject / Authority Key Identifier (chain building) |
| **TBS** | To-Be-Signed portion of a cert |
| **CRL** | Certificate Revocation List |
| **OCSP** | Online Certificate Status Protocol |
| **PEM / DER** | Base64 text / raw binary cert encodings |
| **ACME** | Automated cert issuance protocol (Let's Encrypt, step-ca) |
| **mTLS** | Mutual TLS — both ends present certs |
| **Trust anchor / root store** | The set of root CAs a client believes |

---

### The 60-second recap
A CA binds identity → public key by signing a certificate. Clients that trust the CA's key
trust everything it signs. **Key type + digest** secure the signature; **extensions**
(basicConstraints/keyUsage/EKU/SAN) define what a cert is allowed to do; **the hierarchy**
(root → intermediate → leaf) isolates the crown-jewel signing key; **internal vs external**
comes down to who already has your root in their trust store; and the **CSR workflow** keeps
private keys where they're born. Everything you'll configure on OPNsense and FreeIPA is an
application of these six ideas.
