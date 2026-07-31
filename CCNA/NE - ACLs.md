
# NE - ACLs


## Standard ACLs

- **Works at Layer 3 (Network layer).**
- **Filters only based on source IP address.**
- **Placed close to the destination (but generally on the source side for inbound filtering).**
- **Order matters: rules are tested in sequence; first match applies.**
- **Most permissive rules are placed last, with an explicit "permit" at the end if needed; otherwise, implicitly deny.**
- **Create the ACL first, then apply it to an interface (inbound or outbound).**


### 1. Numbering

- **Standard ACLs:** 1-99 and 1300-1999 (expanded range)

### 2. Base syntax / Variants

- **Permit:**  
    `access-list [number] permit [source] [wildcard mask]`
- **Deny:**  
    `access-list [number] deny [source] [wildcard mask]`
- **Example with network and host:**
    - Network: `access-list 10 permit 192.168.1.0 0.0.0.255`
    - Host: `access-list 10 permit 192.168.1.5 0.0.0.0`


### 3. Apply ACL to interface

- **Inbound:**  
    `ip access-group [ACL number] in`
- **Outbound:**  
    `ip access-group [ACL number] out`

### 4. Remove ACL from interface

- **Remove from interface:**  
    `no ip access-group [ACL number] in`  
    or  
    `no ip access-group [ACL number] out`

### 5. Show access list

- **Display ACLs:**  
    `show access-lists`  
    or  
    `show access-list [number]`

### 6. Delete access list (since modification isn’t possible directly)

- **Delete entire ACL:**  
    `no access-list [number]`








Certainly! Here's a comparison table between **Numbered ACLs** and **Named ACLs**:

| Feature                        | Numbered ACLs                               | Named ACLs                                    |
|------------------------------|------------------------------------------|----------------------------------------------|
| **Identification**            | By **numbers** (e.g., 10, 100, 1300)   | By **names** (e.g., `BLOCK_SALES`)          |
| **Ease of Use**                 | Simpler to create quickly             | Slightly more complex but more descriptive |
| **Readability**                 | Less descriptive, numeric identifiers | More descriptive, readable labels           |
| **Modification**                | Cannot modify rules directly; delete and reconfigure | Can modify rules easily using the name   |
| **Management**                  | Suitable for small/simple networks    | Ideal for complex/large networks           |
| **Support for Standard ACLs**   | Yes (1-99, 1300-1999)                | Yes (by name)                              |
| **Support for Extended ACLs**   | Yes (100-199, 2000-2699)            | Yes (by name)                              |
| **Syntax Example**              | `access-list 10 permit 192.168.1.0 0.0.0.255` | `ip access-list standard MY_ACL`<br>`permit 192.168.1.0 0.0.0.255` |
| **Deleting/Removing**           | `no access-list [number]`            | `no ip access-list [name]`                |

---

Would you like a quick reference or examples of how to create each type?


### Base Syntax for Named Standard ACLs with Sequence Numbers

          
          
          `ip access-list standard <name> [sequence <seq-number>] {permit | deny} <ip-addr> [wildcard <wildcard-mask>] | host <ip-addr>`


### Explanation of Components:

|Part|Description|
|---|---|
|`ip access-list standard`|Command to define a standard ACL (standard vs extended)|
|`<name>`|The name of the ACL (choose a descriptive name)|
|`[sequence <seq-number>]`|Optional; sequence number for ordering and easy management|
|`{permit|deny}`|
|`<ip-addr>`|Source IP address|
|`[wildcard <wildcard-mask>]`|Optional; wildcard mask for specifying IP range|
|`host <ip-addr>`|To specify a single host IP address (equivalent to IP + wildcard 0.0.0.0)|

---
1. **Basic ACL with sequence number:**

          
          
          `ip access-list standard MY_ACL sequence 10 permit 192.168.1.0 0.0.0.255`
      

2. **Adding a deny with a specific host:**

          
          
          `ip access-list standard MY_ACL sequence 20 deny host 10.0.0.1`
      

3. **Without sequence number (automatic sequence):**

          
          
          `ip access-list standard MY_ACL permit 172.16.0.0 0.0.255.255`
      

---

### Notes:

- Using sequence numbers with gaps (like 10, 20, 30) makes it easier to insert or delete rules later.
- You can **add rules** to the ACL by entering global configuration mode and using the above syntax.
- To **apply the ACL** to an interface:

          
          
          `interface GigabitEthernet0/1  ip access-group MY_ACL in`