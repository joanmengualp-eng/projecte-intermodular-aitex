<h1 align="center">📦 Tasca 5. Selecció de recursos materials i humans</h1>

<p align="center">
  <img alt="Fase" src="https://img.shields.io/badge/Fase-Planificaci%C3%B3-1F3A5F?style=for-the-badge">
  <img alt="Projecte" src="https://img.shields.io/badge/Projecte-Aitex_Datacenter-2E7D6B?style=for-the-badge">
  <img alt="CE" src="https://img.shields.io/badge/Criteris_Avaluaci%C3%B3-RA2.c_·_RA2.g-2E7D6B?style=for-the-badge">
</p>

Aquest document concreta la selecció raonada de **models comercials verificables de maquinari**, **infraestructura elèctrica/tèrmica**, **programari i llicències** (100 % alineats amb [`04-maquinari-programari.md`](../04-maquinari-programari.md) i [`06-pressupost.md`](06-pressupost.md)) i els **perfils professionals d'enginyeria** per a la implantació d'**Aitex Datacenter**.

---

## 💻 1. Selecció de Recursos Materials: Maquinari i Infraestructura Física

Tots els equips s'han dimensionat segons els càlculs de potència (96,4 kW mitjans / 138,2 kW pic) i redundància **N+1** validats a la Tasca 2:

| Categoria / Element | Model Seleccionat | Quantitat | Requisits Principals i Justificació Tècnica | Fitxa Tècnica / Font |
|---|---|:---:|---|---|
| **Encaminadors de Vora (Edge)** | **MikroTik CCR2216-1G-12XS-2XQ** | 2 u. | CPU 16 nuclis, 12×25G SFP28 + 2×100G QSFP28, L3 Hardware Offload, BGP complet i doble font *hot-swap*. | [Fitxa MikroTik](https://mikrotik.com/product/ccr2216_1g_12xs_2xq) |
| **Tallafocs Perimetrals (NGFW)** | **Fortinet FortiGate 400F** | 2 u. | 12 Gbps IPS / 9 Gbps Threat Protection, acceleració NP7/CP9, clúster HA FGCP i doble font *hot-swap*. | [Datasheet Fortinet](https://www.fortinet.com/resources/data-sheets/fortigate-400f-series) |
| **Commutadors de Nucli (Core L3)** | **FS S5860-20SQ** | 2 u. | 20×10G SFP+ + 4×25G SFP28 + 2×40G QSFP+, 760 Gbps, MLAG/Stacking L3 i doble font *hot-swap*. | [Fitxa FS.com](https://www.fs.com/products/207189.html) |
| **Commutadors Top-of-Rack (ToR)** | **FS S3900-48T4S** | 32 u. | 48×1G RJ45 + 4×10G SFP+ uplinks, 802.1Q, LACP. 2 unitats per cada armari rack de sala blanca. | [Datasheet FS S3900](https://img-en.fs.com/file/datasheet/s3900-series-switches-datasheet.pdf) |
| **Commutadors d'Oficina (PoE+)** | **Cisco CBS350-48P-4G** | 2 u. | 48 ports Gigabit PoE+ (370 W) + 4×SFP. Alimenten els 6 APs Wi-Fi i els 16 telèfons VoIP. | [Datasheet Cisco](https://www.cisco.com/c/en/us/products/collateral/switches/business-350-series-managed-switches/datasheet-c78-744156.html) |
| **Punts d'Accés Wi-Fi 6** | **Ubiquiti UniFi U6 Pro** | 6 u. | Wi-Fi 6 4×4 MIMO, 5,3 Gbps agregats, PoE (13 W), multi-SSID WPA3-Enterprise + portal visites. | [Specs Ubiquiti](https://techspecs.ui.com/unifi/wifi/u6-pro) |
| **Servidors de Producció (1U)** | **Dell PowerEdge R360** | 320 u. | 1U mono-socket Xeon E-2400, 64–128 GB DDR5 ECC, 2×NVMe RAID-1, doble PSU 600W *hot-swap*, iDRAC9. | [Documentació Dell](https://www.dell.com/support/product-details/en-us/product/poweredge-r360/resources/manuals) |
| **Servidors Gestió Interna (2U)** | **Dell PowerEdge R750xs** | 7 u. | 2U Xeon Scalable, 64–128 GB ECC, emmagatzematge RAID-6 SAS/NVMe, doble PSU, iDRAC9 (AD, SIEM, BMS, Backup, Web). | [Specsheet Dell](https://i.dell.com/sites/csdocuments/Product_Docs/en/R750xs-specsheet.pdf) |
| **Armaris Rack 42U i PDUs A+B** | **Rittal TS IT 42U** + **APC AP8853** | 18 racks / 36 PDUs | 16 racks en 4 sales blanques (20 srv/rack) + 2 racks en Planta 1; 2 PDUs gestionables (A+B) per rack. | [Rittal TS IT](https://www.rittal.com/com-en/products/PG0002SCHRANK1/PG0003SCHRANK1/PG0004SCHRANK1) |
| **Consoles KVM sobre IP (1U)** | **Aten CL5708IM** | 8 u. | Consola LCD 17" 1U amb accés KVM-over-IP xifrat a la BIOS/OOBM (2 per sala blanca). | [Fitxa Aten](https://www.aten.com/global/en/products/kvm/lcd-kvm-switches/cl5708i/) |
| **SAIs Trifàsics N+1 (80 kW)** | **Schneider Galaxy VS 80 kW (GVSUPS80KHS)** | 3 u. | 400V trifàsic On-Line doble conversió; 2×80 kW actius (160 kW útils > 138,2 kW pic) + 1 redundant (N+1), 15–20 min bateria. | [Fitxa Schneider](https://www.se.com/us/en/product/GVSUPS80KHS/galaxy-vs-ups-80kw-400v-for-external-batteries-startup-5x8/) |
| **Grup Electrògen Dièsel** | **Generador 250 kVA Insonoritzat + ATS** | 1 u. | Arrancada automàtica en < 30 s, cobrint el pic total del CPD (222,7 kVA) amb autonomia de 48 h. | *Normativa Tier II/III* |
| **Climatització Precisió (CRAC)** | **Schneider Uniflair 35–40 kW** | 8 u. | 2 unitats per cada sala blanca en **N+1**; una sola unitat cobreix el 100 % del pic tèrmic per planta (32,8 kW). | [Gamma Uniflair](https://www.se.com/ww/en/work/products/master-ranges/uniflair/) |
| **Estacions Fixes (PC) i Monitors** | **PC Core i7 / 32 GB** + **Dell U2722D (27")** | 13 PCs / 26 mon. | Estacions multipantalla per al NOC/SOC, Sistemes i Administració. | [Dell UltraSharp](https://www.dell.com/en-us/shop/dell-ultrasharp-27-monitor-u2722d/apd/210-ayze/monitors-monitor-accessories) |
| **Portàtils Tècnics Robustos** | **Dell Latitude 5540 (i7)** | 5 u. | 3 per a Manteniment físic i 2 compartits per a Sistemes en mobilitat per les 5 plantes. | [Dell Latitude](https://www.dell.com/support/product-details/en-us/product/latitude-15-5540-laptop/overview) |
| **Videowall NOC, VoIP i Impressores**| **Samsung VH55R-R (×4)**, **Yealink T43U (×16)**, **HP M528 (×1)**, **Brother PT-E550W (×2)** | Lot complet | Visualització 24/7 al NOC, telefonia PoE per a la plantilla, impressió legal i etiquetatge TIA-606-B. | [Samsung](https://displaysolutions.samsung.com/digital-signage/detail/1673/VH55R-R) · [Brother](https://www.brother.es/impresoras-de-etiquetas/pt-e550wvp) |

---

## 🧩 2. Selecció de Recursos Materials: Programari i Llicenciament

El llicenciament està estrictament unificat entre tots els documents del projecte (sense discrepàncies de quantitats ni edicions):

| Categoria | Programari Seleccionat | Quantitat / Llicències | Tipus de Llicència | Funció Principal i Font |
|---|---|:---:|---|---|
| **Hipervisor de Virtualització** | **Proxmox VE** *(Subscripció Community)* | **320 sockets** *(1 per servidor R360)* | Open Source AGPLv3 + Subscripció anual repositori Enterprise | Virtualització *bare-metal* per a *Half/Full-Server* i migració en viu. [Preus Proxmox](https://www.proxmox.com/en/products/proxmox-virtual-environment/pricing) |
| **SO Servidors Domini (AD)** | **Windows Server 2022 Datacenter** | **2 llicències** *(16 cores cadascuna)* | Perpètua (Microsoft Commercial) | Controladors de Domini (`srv-domini-1` i `srv-domini-2`), DNS/DHCP i GPO. [Microsoft](https://www.microsoft.com/en-us/windows-server/pricing) |
| **SO Servidors Linux Interns/DMZ** | **Debian GNU/Linux 12 (Bookworm)** | **5 servidors físics + 2 VMs DMZ** | Codi Obert (GPL, 0 €) | `srv-web`, `srv-monitor`, `srv-bms`, `srv-backup-1/2`, `srv-correu` i `srv-dns-public`. [Debian](https://www.debian.org/) |
| **SO Equips d'Usuari** | **Windows 11 Pro** *(+ dual boot Ubuntu LTS en tècnics)* | **18 llicències** *(13 PC + 5 portàtils)* | OEM / Retail | Sistema operatiu corporatiu per als 18 equips de la plantilla. |
| **Seguretat Perimetral Firewall** | **FortiGuard UTP + FortiCare 24×7** | **2 subscripcions** *(1 per FortiGate 400F)* | Subscripció anual | Signatures IPS, anti-malware, filtratge web i suport de reemplaçament 24/7. |
| **Còpies de Seguretat** | **Veeam Backup & Replication** | **1 suite clúster intern** | Subscripció anual | Còpies immutables dels 7 servidors interns i snapshots crítics. [Veeam](https://www.veeam.com/) |
| **Seguretat Endpoint (EDR)** | **Bitdefender GravityZone Business** | **18 llicències** *(1 per equip usuari)* | Subscripció anual | Protecció anti-ransomware i EDR per als 13 PCs i 5 portàtils. [Bitdefender](https://www.bitdefender.com/business/) |
| **SIEM, Monitoratge, BMS, ERP i Ofimàtica** | **Zabbix + Grafana + Wazuh · Odoo Community · LibreOffice / Thunderbird** | **Ilimitat (16 usuaris / 373 nodes)** | Codi Obert (0 € en llicència) | Monitoratge 24/7, telemetria IoT/BMS, ERP/Facturació/Tíquets i ofimàtica corporativa. |

---

## 👥 3. Selecció de Recursos Humans: Equip d'Implantació del Projecte

> [!IMPORTANT]
> **Distinció clau de rols:** Els perfils següents formen **l'Equip Tècnic d'Implantació i Posada en Marxa** (enginyeria i muntatge inicial). No s'han de confondre amb els **16 treballadors fixos d'Aitex Datacenter** que operaran el CPD diàriament una vegada inaugurat.

| Perfil de l'Equip d'Implantació | Hores / Dedicació | Responsabilitats i Tasques Assignades |
|---|:---:|---|
| 📐 **Cap de Projecte (Project Manager / Scrum Master)** | 80 h | Direcció Scrum/Trello, control pressupostari, validació d'entregables i coordinació de proveïdors. |
| 🌐 **Arquitecte de Xarxes (Network Engineer)** | 140 h | Configuració d'eBGP/VRRP als routers `CCR2216`, clúster FGCP `FortiGate 400F`, MLAG 40G i les **11 VLANs** als switches Core i ToR. |
| 🐧 **Enginyer de Sistemes i Ciberseguretat** | 140 h | Desplegament automatitzat de Proxmox VE als 320 servidors, Active Directory, DMZ, Veeam, Zabbix/Wazuh i bastionat (*hardening*). |
| 🔧 **Tècnics Especialistes en CPD i Cablejat** | 320 h | Muntatge dels 18 racks, instal·lació dels 327 servidors i 32 switches ToR, fibra OM4/Cat6A, etiquetatge TIA-606-B i PDUs A+B. |
| ⚡ **Enginyeria Elèctrica i Climatització (Certificació)** | *Projecte clau en mà* | Posada en marxa i certificació legal del centre de transformació 250 kVA, 3×SAIs 80 kW, grup electrògen, 8×CRAC Uniflair i gas inert. |
