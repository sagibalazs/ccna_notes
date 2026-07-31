## To do list

- pro und contra wifi, vs cabel
-ssid - service set identifier
- wpa2/3 - wifi protected access (wpa2 with unique Advancde Encryption  Standard  - AES)(with Temporal Kye Integrity Protocol just 54Mbit /s möglich)
- wpa2 with Counter-Mode/CBC-Mac Protocol CCMP - look after with which encryption what a speed is available
- encryption Wired Equivalent Privacy  WEP 
- Spread - Sprectrum (variabel frequency for data transfer during the communication, microwave range)
- Frequency Hopping Spread Sprectrum FHSS - 2,4 GHz band, 79 canal each with 1MHz, random ordering the canals 1600 mal pro sec
- DSSS - Direct Sequence Spread Spectrum  - Pseudo -Noise - Military
- OFDM - Orthogonal Requency Division Mltiplex 
- ISM-Band - Industrial, Scientific and Medical (sometimes named ISMO, O stence for Office) - free from charge and permissions
- monomode and mulitmode fasern
- RF - broadcasting, >> how to catch it ? what the name of the protocol or proccess, where the devices looking for broadcast ? (wardiriving to catch)
- 
# Preface

- this part of he CCNA Based on Todd Lammle - CompTIA Network+ Guide (5th Edition)
# Links and Sources

- HERD IT Fachliteratur - Netzwerke Grundlagen 1. Ausgabe 12/2021, ISBN 978-3-98569-047-3
- 

Agency Purpose Website
Institute of Electrical and Elec-
tronics Engineers (IEEE)
Creates and maintains operational
standards
www.ieee.org
Federal Communications
Commission (FCC)
Regulates the use of wireless
devices in the US
www.fcc.gov
European Telecommunications
Standards Institute (ETSi)
Chartered to produce common
standards in Europe
www.etsi.org
Wi- Fi Alliance Promotes and tests for WLAN inter-
operability
www.wi-fi.org
WLAN Association (WLANA) Educates and raises consumer
awareness regarding WLANs
www.wlana.org

## Introduction Wireless Technology

- 802.11 specifications, international not regulated, free to produce, sell, use, but obligatory, that any device should be able to operate without configuring much, if anything at all. 
- half duplex, two way communication which use Radio Frequencies #RF 
- antenna radiates RFs, 
	- this can be absorved, refracted or reflected by many objects like walls, water, metal surfaces resulting low signal streng
	- its simple sensitive for the environmental factors
- increasing the frequency, we can reach a higher transfer rates, but it will reduce the range of the transmitting distances
- lower the frequency, the transfer date is decreasing, but the transmitting range is increased

### Wireless Agencies and Standards

Agency Purpose Website
Institute of Electrical and Elec-
tronics Engineers (IEEE)
Creates and maintains operational
standards
www.ieee.org
Federal Communications
Commission (FCC)
Regulates the use of wireless
devices in the US
www.fcc.gov
European Telecommunications
Standards Institute (ETSi)
Chartered to produce common
standards in Europe
www.etsi.org
Wi- Fi Alliance Promotes and tests for WLAN inter-
operability
www.wi- fi.org
WLAN Association (WLANA) Educates and raises consumer
awareness regarding WLANs
www.wlana.org


### Frequencies

- since using radio frequencies, its regulated by the same governs and bodies like AM/FM radios (and laws)
- 


| Co  |     |
| --- | --- |
|     |     |

- using ranges outside of these bands down below, need a special license to do so

![[Pasted image 20250417171122.png]]

![[Pasted image 20251207234529.png]]

|**Year**|**Standard**|**Max Speed (Theoretical)**|**New Features**|**Description**|
|---|---|---|---|---|
|1997|**802.11**|2 Mbps|Original specification (2.4 GHz)|The original standard, now obsolete, using FHSS or DSSS.|
|1999|**802.11a**|54 Mbps|5 GHz band, **OFDM**|Uses the **5 GHz** band and Orthogonal Frequency-Division Multiplexing (OFDM) for higher speeds and less interference.|
|1999|**802.11b**|11 Mbps|2.4 GHz band, HR-DSSS|Uses the **2.4 GHz** band and High-Rate DSSS (HR-DSSS). Widely adopted but susceptible to interference.|
|2003|**802.11g**|54 Mbps|2.4 GHz band, **OFDM**|Combines the speed of 802.11a with the frequency of 802.11b (**2.4 GHz**), using OFDM.|
|2009|**802.11n** (Wi-Fi 4)|600 Mbps|**MIMO**, **channel bonding** (40 MHz)|Introduced **Multiple-Input Multiple-Output (MIMO)** and channel bonding to significantly increase throughput in both 2.4 GHz and 5 GHz bands.|
|2013|**802.11ac** (Wi-Fi 5)|6.9 Gbps|**MU-MIMO**, wider channels (up to 160 MHz)|Operates exclusively in the **5 GHz** band. Introduced **Multi-User MIMO (MU-MIMO)** for better efficiency.|
|2019|**802.11ax** (Wi-Fi 6)|9.6 Gbps|**OFDMA**, 1024-QAM, target wake time|Uses Orthogonal Frequency-Division Multiple Access (**OFDMA**) for better efficiency. Operates in 2.4 GHz, 5 GHz, and the new **6 GHz** (for Wi-Fi 6E) bands.|
|2024 (Expected)|**802.11be** (Wi-Fi 7)|40 Gbps|**EHT** (Extremely High Throughput), 320 MHz channels, Multi-Link Operation|Focuses on Extremely High Throughput (**EHT**) using all three bands (2.4, 5, and 6 GHz) simultaneously for multi-link operation (MLO).|

### Cellular Technologies

#### GSM - Global System Mobile

- cell phone that use #SIM chip, that is contain all subscriber data
- SIM is mandatory to use the network
- cell phone cloning as a danger, where copy of the SIM allow for another user to identify yourself as owner, and execute actions on a name of him
- secret key cryptography is used by authentication between the phone and network provider
- 

#### FDMA - Frequency-division multiple access

- modulation technique, which used in cellular wireless networks, its divide the frequencies into bands, and assign these to users
- used in 1G networks

#### TDMA - Time-divison multiple access

- increase the speed of the FDMA, by dividing the channels into time slots and assigning slots to calls
- its also helps to preventing eavesdropping  in calls

#### CDMA - Code-division multiple access

- assigns a unique code to each call or transmissions and spreads the data across the spectrum, allowing a call to make use of all frequencies

#### 3G - Third generation cellular data network

- first time deliver usable data speed, in 1990 its 2 Mbps
- handled phone calls, MMS etc
- after 3G was arrived, many other data format was became accessible like HTML, videos, music

#### 4G - fourth generation cellular data network

- its pushed up to 100 Mbps up to 1 Gbps

#### LTE - Long-Term Evolution

- also called 4G LTE

#### 5G - Fith generation cellular network

- its up to 100x faster than 4G

#### Summary of Cellular Technologies

| Technology | Deployment | Bandwidth        | Standards            | Technology          |
| ---------- | ---------- | ---------------- | -------------------- | ------------------- |
| 3G         | 1990       | 2 Mbps           | WCDMA, CDMA- 2000    | CDMA/IP             |
| 4G         | 2000       | 200 to 1000 Mbps | CDMA, LTE, WiMAX     | Unified IP, LAN/WAN |
| 5G         | 2014       | 1 to 10 Gbps     | OFDM, MIMO, nm Waves | Unified IP, LAN/WAN |

### The 802.11 Standards

















---
--- 
## ISM - Frequencies table (canals, frequency intervals, evolution, speed, encryptions, using the channels)


units and "Dämpung"
## Standardization Organisations

IEEE 

### Standards 802.11

table with all 802.11 characteristic (speed, channels

- MIMO - Multiple Input Mul. Output (2,4 and 5GHz simultan)
- MU-MIMO Multi-User ....
- Dymanic Frequency Selection- automatic change the channel in 5GHz 
- TPC - Transmit Power Control
- UMTS - Shared Media

## Extending the WLAN Networks

- Wlan REpeater- Powerline Products
- Wlan basis - Access Point
- WLAN - Router
- WLAN Adapter (Bridge)
- Wifi  802.11z AD-HOC WLAN for printers , mobiles, cameras etc. 
- Mesh WLAN - like a switch, forwarded 

## Bluetooth

- cheap possibility in nearfeld communication
- table with all bluetooth variant, speed, 

## NFC and RFID

- near feld communication
- radio frequency identification 

## WPAN for short distance

## IOT

## LoRa - Low Power Wide Area Network

for energy providers to read the power meters up to 50km  distance

## Licht und Laser

### Infrarot 

### Laser

## Types of Wireless

