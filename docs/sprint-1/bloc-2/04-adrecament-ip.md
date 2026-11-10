<h1 align="center">🗺️ Tasca 4. Pla d'adreçament IP i noms dels equips</h1>

<p align="center">
  <img alt="Fase" src="https://img.shields.io/badge/Fase-Disseny_T%C3%A8cnic-D97706?style=for-the-badge">
  <img alt="Projecte" src="https://img.shields.io/badge/Projecte-Aitex_Datacenter-2E7D6B?style=for-the-badge">
  <img alt="CE" src="https://img.shields.io/badge/Criteris_Avaluaci%C3%B3-RA2.e_·_RA2.i-2E7D6B?style=for-the-badge">
</p>

Aquest document defineix el pla exhaustiu d'adreçament IP privat sota la xarxa base **`192.168.0.0/16`**, la segmentació en xarxes virtuals (**VLANs**), la nomenclatura simplificada i directa dels equips (*Hostnames*) i la justificació de seguretat entre la xarxa interna i la **DMZ** per a **Aitex Datacenter**.

---

## 📐 1. Esquema General de Subxarxes i VLANs (`192.168.0.0/16`)

S'ha adoptat l'espai d'adreçament privat **RFC 1918** **`192.168.0.0/16`**, dividit jeràrquicament en blocs `/24` (fins a 254 equips per subxarxa) on **el tercer octet de la IP coincideix de manera directa amb l'identificador de la VLAN (`192.168.[VLAN].0/24`)**, facilitant al màxim la gestió i el diagnòstic:

| VLAN ID | Nom de la Subxarxa | Xarxa (CIDR) | Màscara de Subxarxa | Passarel·la (Gateway) | Rang d'adreces útils | Tipus d'assignació | Funció / Àrea |
|:---:|---|---|---|---|---|:---:|---|
| **—** | *Trànsit R1 - FW1* | `192.168.0.0/30` | `255.255.255.252` | `192.168.0.1` | `192.168.0.1 - 192.168.0.2` | Estàtica | Enllaç punt a punt Router 1 i Firewall 1 |
| **—** | *Trànsit R2 - FW2* | `192.168.0.4/30` | `255.255.255.252` | `192.168.0.5` | `192.168.0.5 - 192.168.0.6` | Estàtica | Enllaç punt a punt Router 2 i Firewall 2 |
| **—** | *Sincronització FW HA*| `192.168.0.8/30` | `255.255.255.252` | `—` | `192.168.0.9 - 192.168.0.10` | Estàtica | Heartbeat i estat del clúster de tallafocs |
| **—** | *Enllaç Inter-Core (LACP)*| `192.168.0.12/30`| `255.255.255.252` | `—` | `192.168.0.13 - 192.168.0.14`| Estàtica | Trànsit troncal entre Switches Core 1 i 2 |
| **10** | **VLAN_GESTIO** | `192.168.10.0/24` | `255.255.255.0` | `192.168.10.1` | `192.168.10.1 - 192.168.10.254` | Estàtica | Gestió tècnica OOBM: IPMI, iLO, KVM i switches |
| **20** | **VLAN_DMZ** | `192.168.20.0/24` | `255.255.255.0` | `192.168.20.1` | `192.168.20.1 - 192.168.20.254` | Estàtica | Serveis amb accés públic des d'Internet (Web/ERP, DNS, Mail) |
| **30** | **VLAN_SERVIDORS_INTERNS**| `192.168.30.0/24` | `255.255.255.0` | `192.168.30.1` | `192.168.30.1 - 192.168.30.254` | Estàtica | Controladors de Domini (AD), Backups corporatius, SIEM/Syslog |
| **40** | **VLAN_OFICINA** | `192.168.40.0/24` | `255.255.255.0` | `192.168.40.1` | `192.168.40.1 - 192.168.40.254` | DHCP (`.50 - .200`) | PCs sobretaula d'Administració, Direcció, Suport i SOC |
| **50** | **VLAN_WIFI_CORP** | `192.168.50.0/24` | `255.255.255.0` | `192.168.50.1` | `192.168.50.1 - 192.168.50.254` | DHCP (`.20 - .250`) | Portàtils tècnics corporatius (WPA3-Enterprise) |
| **60** | **VLAN_WIFI_VISITES** | `192.168.60.0/24` | `255.255.255.0` | `192.168.60.1` | `192.168.60.1 - 192.168.60.254` | DHCP (`.10 - .250`) | Accés a Internet exclusiu per a visites amb portal captiu |
| **70** | **VLAN_CONTROL_IOT** | `192.168.70.0/24` | `255.255.255.0` | `192.168.70.1` | `192.168.70.1 - 192.168.70.254` | Estàtica / DHCP | Sensors de temperatura, humitat, SAIs, Videowall NOC i càmeres |
| **101** | **VLAN_PROD_PLANTA2** | `192.168.101.0/24`| `255.255.255.0` | `192.168.101.1`| `192.168.101.1 - 192.168.101.254`| Estàtica | Sala Blanca 1: 80 servidors de producció clients B2B |
| **102** | **VLAN_PROD_PLANTA3** | `192.168.102.0/24`| `255.255.255.0` | `192.168.102.1`| `192.168.102.1 - 192.168.102.254`| Estàtica | Sala Blanca 2: 80 servidors de producció clients B2B |
| **103** | **VLAN_PROD_PLANTA4** | `192.168.103.0/24`| `255.255.255.0` | `192.168.103.1`| `192.168.103.1 - 192.168.103.254`| Estàtica | Sala Blanca 3: 80 servidors de producció clients B2B |
| **104** | **VLAN_PROD_PLANTA5** | `192.168.104.0/24`| `255.255.255.0` | `192.168.104.1`| `192.168.104.1 - 192.168.104.254`| Estàtica | Sala Blanca 4: 80 servidors de producció clients B2B |

---

## 🏷️ 2. Estàndard de Nomenclatura Directa i Simplificada

S'aplica un criteri de nomenclatura natural, intuïtiu i clar per al projecte:

$$\text{[tipus-dispositiu]} - \text{[funció o lloc]} - \text{[número]}$$

* **Noms directes i autoexplicatius:**
  * Routers: `router-1`, `router-2`
  * Firewalls: `firewall-1`, `firewall-2` (Passarel·la Virtual DMZ: `firewall-vip-dmz`)
  * Switches de Nucli: `switch-core-1`, `switch-core-2`
  * Switches d'Oficina: `switch-oficina-1`, `switch-oficina-2`
  * Switches ToR: `switch-tor-p2-01` .. `08`, `switch-tor-p3-01` .. `08`, etc.
  * Servidors: `srv-web`, `srv-correu`, `srv-domini-1`, `srv-backup-1`, etc.
  * PCs: `pc-direccio`, `pc-admin`, `pc-sistemes-1` .. `6`, `pc-soc-1` .. `4`
  * Portàtils: `portatil-sistemes-1` .. `2`, `portatil-manteniment-1` .. `3`
  * Servidors de Clients: `srv-client-p2-01` .. `80` (repartits per planta)
* **Domini intern corporatiu:** `intra.aitexdatacenter.com`

---

## 💻 3. Taula d'Adreçament d'Equips i Dispositius de Xarxa

### A. Electrònica de Xarxa, Seguretat i Enllaços de Trànsit

| Nom de l'Equip (*Hostname*) | Xarxa / VLAN | Adreça IP | Màscara / Prefix | Passarel·la (*Gateway*) | Descripció i Funció |
|---|---|:---:|:---:|:---:|---|
| `router-1` (WAN) | Trànsit ISP 1 | `203.0.113.2` | `255.255.255.252` (`/30`) | `203.0.113.1` | Router de Vora 1 (BGP Primari, Fibra ISP 1) |
| `router-1` (LAN) | Trànsit R1-FW1 | `192.168.0.1` | `255.255.255.252` (`/30`) | `—` | Pota interna cap al Firewall 1 |
| `router-2` (WAN) | Trànsit ISP 2 | `198.51.100.2` | `255.255.255.252` (`/30`) | `198.51.100.1` | Router de Vora 2 (BGP Secundari, Fibra ISP 2) |
| `router-2` (LAN) | Trànsit R2-FW2 | `192.168.0.5` | `255.255.255.252` (`/30`) | `—` | Pota interna cap al Firewall 2 |
| `firewall-1` (WAN) | Trànsit R1-FW1 | `192.168.0.2` | `255.255.255.252` (`/30`) | `192.168.0.1` | Firewall Primari Actiu (Interfície WAN) |
| `firewall-2` (WAN) | Trànsit R2-FW2 | `192.168.0.6` | `255.255.255.252` (`/30`) | `192.168.0.5` | Firewall Secundari Passiu (Interfície WAN) |
| `firewall-1` (HA Sync)| Sincronització | `192.168.0.9` | `255.255.255.252` (`/30`) | `—` | Port de sincronització Clúster HA |
| `firewall-2` (HA Sync)| Sincronització | `192.168.0.10` | `255.255.255.252` (`/30`) | `—` | Port de sincronització Clúster HA |
| `firewall-vip-dmz` | VLAN 20 (DMZ) | `192.168.20.1` | `255.255.255.0` (`/24`) | `—` | IP Virtual Clúster Firewall per a la DMZ (Gateway) |
| `switch-core-1` | VLAN 10 (Gestió) | `192.168.10.2` | `255.255.255.0` (`/24`) | `192.168.10.1` | Switch Core 1 L3 (Gestió i Inter-VLAN Routing) |
| `switch-core-2` | VLAN 10 (Gestió) | `192.168.10.3` | `255.255.255.0` (`/24`) | `192.168.10.1` | Switch Core 2 L3 (Stacking/VSS amb Core 1) |
| `switch-oficina-1` | VLAN 10 (Gestió) | `192.168.10.4` | `255.255.255.0` (`/24`) | `192.168.10.1` | Switch PoE Planta 1 (Oficina i Punts d'Accés) |
| `switch-oficina-2` | VLAN 10 (Gestió) | `192.168.10.5` | `255.255.255.0` (`/24`) | `192.168.10.1` | Switch PoE Planta 1 Redundant (Telèfons IP) |
| `switch-tor-p2-01` a `08` | VLAN 10 (Gestió) | `192.168.10.11` - `.18` | `255.255.255.0` (`/24`) | `192.168.10.1` | 8 Switches ToR Planta 2 (Sala Blanca 1) |
| `switch-tor-p3-01` a `08` | VLAN 10 (Gestió) | `192.168.10.19` - `.26` | `255.255.255.0` (`/24`) | `192.168.10.1` | 8 Switches ToR Planta 3 (Sala Blanca 2) |
| `switch-tor-p4-01` a `08` | VLAN 10 (Gestió) | `192.168.10.27` - `.34` | `255.255.255.0` (`/24`) | `192.168.10.1` | 8 Switches ToR Planta 4 (Sala Blanca 3) |
| `switch-tor-p5-01` a `08` | VLAN 10 (Gestió) | `192.168.10.35` - `.42` | `255.255.255.0` (`/24`) | `192.168.10.1` | 8 Switches ToR Planta 5 (Sala Blanca 4) |
| `ap-oficina-1` - `2` | VLAN 10 (Gestió) | `192.168.10.51` - `.52` | `255.255.255.0` (`/24`) | `192.168.10.1` | Punts d'Accés Wi-Fi Planta 1 (Oficines) |
| `ap-planta-2` a `5` | VLAN 10 (Gestió) | `192.168.10.53` - `.56` | `255.255.255.0` (`/24`) | `192.168.10.1` | Punts d'Accés Wi-Fi Sales Blanques (Plantes 2 a 5) |

---

### B. Serveis en DMZ i Servidors Interns (7 Servidors Físics 2U + Instàncies Virtuals)

> [!NOTE]
> Els **7 servidors físics de gestió interna** (Rack 2U) allotgen els serveis fonamentals d'administració, mentre que els serveis de passarel·la pública a la DMZ (`srv-correu` i `srv-dns-public`) s'executen com a instàncies virtualitzades aïllades amb interfície exclusiva a la `VLAN 20`.

| Nom de l'Equip (*Hostname*) | Xarxa / VLAN | Adreça IP | Màscara / Prefix | Passarel·la (*Gateway*) | Descripció i Serveis |
|---|---|:---:|:---:|:---:|---|
| `srv-web` | VLAN 20 (DMZ) | `192.168.20.10` | `255.255.255.0` (`/24`) | `192.168.20.1` | **[Servidor Físic #1]** Portal Web corporatiu, Facturació i ERP B2B (HTTPS) |
| `srv-correu` *(VM)* | VLAN 20 (DMZ) | `192.168.20.20` | `255.255.255.0` (`/24`) | `192.168.20.1` | Relay de Correu / Passarel·la Anti-Spam i Anti-Malware (SMTP) |
| `srv-dns-public` *(VM)* | VLAN 20 (DMZ) | `192.168.20.30` | `255.255.255.0` (`/24`) | `192.168.20.1` | Servidor DNS Autoritatiu Públic per al domini comercial |
| `srv-domini-1` | VLAN 30 (Interns)| `192.168.30.10` | `255.255.255.0` (`/24`) | `192.168.30.1` | **[Servidor Físic #2]** Controlador de Domini Primari (Active Directory, DNS/DHCP intern) |
| `srv-domini-2` | VLAN 30 (Interns)| `192.168.30.11` | `255.255.255.0` (`/24`) | `192.168.30.1` | **[Servidor Físic #3]** Controlador de Domini Secundari (Rèplica i Alta Disponibilitat) |
| `srv-monitor` | VLAN 30 (Interns)| `192.168.30.20` | `255.255.255.0` (`/24`) | `192.168.30.1` | **[Servidor Físic #4]** Servidor SIEM i Monitoratge Zabbix / Grafana (Syslog/SNMP) |
| `srv-bms` | VLAN 30 (Interns)| `192.168.30.25` | `255.255.255.0` (`/24`) | `192.168.30.1` | **[Servidor Físic #5]** Plataforma BMS (Building Management) i Telemetria de Sensors |
| `srv-backup-1` | VLAN 30 (Interns)| `192.168.30.30` | `255.255.255.0` (`/24`) | `192.168.30.1` | **[Servidor Físic #6]** Servidor de Còpies de Seguretat Primari (Veeam / Proxmox Backup) |
| `srv-backup-2` | VLAN 30 (Interns)| `192.168.30.31` | `255.255.255.0` (`/24`) | `192.168.30.1` | **[Servidor Físic #7]** Cabina d'Emmagatzematge NAS/SAN, Fitxers i rèplica de snapshots |
| `srv-fitxers` *(VIP)* | VLAN 30 (Interns)| `192.168.30.40` | `255.255.255.0` (`/24`) | `192.168.30.1` | IP de Servei de Recurs Compartit (SMB/NFS sobre `srv-backup-2`) per a contractes i ISOs |

---

### C. Equips Clients de la Plantilla, Perifèrics i Servidors de Producció

| Nom de l'Equip (*Hostname*) | Xarxa / VLAN | Adreça IP | Màscara / Prefix | Passarel·la (*Gateway*) | Descripció del lloc de treball |
|---|---|:---:|:---:|:---:|---|
| `pc-direccio` | VLAN 40 (Oficina) | `192.168.40.10` | `255.255.255.0` (`/24`) | `192.168.40.1` | Lloc de Direcció General (CEO) |
| `pc-admin` | VLAN 40 (Oficina) | `192.168.40.11` | `255.255.255.0` (`/24`) | `192.168.40.1` | Responsable d'Administració, RRHH i Finances |
| `pc-comercial` | VLAN 40 (Oficina) | `192.168.40.12` | `255.255.255.0` (`/24`) | `192.168.40.1` | Comercial B2B i Màrqueting |
| `pc-sistemes-1` a `6` | VLAN 40 (Oficina) | `192.168.40.21` - `.26` | `255.255.255.0` (`/24`) | `192.168.40.1` | 6 Estacions de Sistemes i Suport Tècnic N1/N2 (Doble monitor) |
| `pc-soc-1` a `4` | VLAN 40 (Oficina) | `192.168.40.31` - `.34` | `255.255.255.0` (`/24`) | `192.168.40.1` | 4 Estacions de Treball Xarxes i Ciberseguretat NOC/SOC |
| `portatil-sistemes-1` - `2` | VLAN 50 (Wi-Fi Corp)| `192.168.50.21` - `.22` (DHCP) | `255.255.255.0` (`/24`) | `192.168.50.1` | 2 Portàtils Tècnics de Sistemes (Mobilitat i diagnòstic) |
| `portatil-manteniment-1` a `3`| VLAN 50 (Wi-Fi Corp)| `192.168.50.31` - `.33` (DHCP) | `255.255.255.0` (`/24`) | `192.168.50.1` | 3 Portàtils Robustos (*Rugged*) de Manteniment Físic |
| `impressora-oficina` | VLAN 40 (Oficina) | `192.168.40.50` | `255.255.255.0` (`/24`) | `192.168.40.1` | Impressora multifunció làser departamental |
| `etiquetadora-1` - `2` | VLAN 40 (Oficina) | `192.168.40.51` - `.52` | `255.255.255.0` (`/24`) | `192.168.40.1` | 2 Impressores tèrmiques d'etiquetatge per a racks/cables |
| `controlador-videowall` | VLAN 70 (IoT/BMS) | `192.168.70.10` | `255.255.255.0` (`/24`) | `192.168.70.1` | Controlador gràfic del Videowall (4 pantalles NOC 55") |
| `srv-client-p2-01` a `80` | VLAN 101 (Planta 2) | `192.168.101.10` - `.89` | `255.255.255.0` (`/24`) | `192.168.101.1` | 80 Servidors Físics Sala Blanca 1 (*Half/Full-Server*) |
| `srv-client-p3-01` a `80` | VLAN 102 (Planta 3) | `192.168.102.10` - `.89` | `255.255.255.0` (`/24`) | `192.168.102.1` | 80 Servidors Físics Sala Blanca 2 (*Half/Full-Server*) |
| `srv-client-p4-01` a `80` | VLAN 103 (Planta 4) | `192.168.103.10` - `.89` | `255.255.255.0` (`/24`) | `192.168.103.1` | 80 Servidors Físics Sala Blanca 3 (*Half/Full-Server*) |
| `srv-client-p5-01` a `80` | VLAN 104 (Planta 5) | `192.168.104.10` - `.89` | `255.255.255.0` (`/24`) | `192.168.104.1` | 80 Servidors Físics Sala Blanca 4 (*Half/Full-Server*) |

---

## 🛡️ 4. Justificació de la Separació entre Xarxa Interna i DMZ

La creació d'una **Zona Desmilitaritzada (DMZ - VLAN 20 - `192.168.20.0/24`)** aïllada de la **Xarxa Interna (VLAN 30 i 40)** i de la **Xarxa de Gestió (VLAN 10)** és un requisit fonamental de seguretat:

```
               [ INTERNET (Públic) ]
                         │
                         ▼
             [ CLÚSTER FIREWALL (NGFW) ]
             ┌───────────┴───────────┐
             │ (Filtratge Estricte)  │
             ▼                       ▼
    [ ZONA DMZ (VLAN 20) ]    [ XARXA INTERNA (VLAN 30/40) ]
    • Portal Web (srv-web)    • Directori Actiu (srv-domini)
    • Relay de Correu         • Servidor de Còpies (srv-backup)
    • DNS Públic              • PCs de Treball dels Empleats
    (192.168.20.0/24)         (192.168.30.0 / 40.0)
             │                       ▲
             └───[ BLOQUEJAT ]───────┘ (Default Deny)
```

1. **Principi de Mínima Confiança (*Zero Trust*) i Aïllament:**
   * El servei web (`srv-web`) rep trànsit directe des d'Internet (ports TCP 80/443). Si rep un atac extern, aquest queda confinat exclusivament a la subxarxa `192.168.20.0/24`.
   * **Política de Denegació per Defecte (*Default Deny*):** Cap equip de la DMZ té autorització per obrir connexions cap a la xarxa interna de la seu (`192.168.30.0/24` o `192.168.40.0/24`).

2. **Protecció de les Dades Crítiques:**
   * La xarxa interna custodia el Directori Actiu, les nòmines, la facturació i les còpies de seguretat (Veeam). La separació evita moviments laterals de malware o atacs de *ransomware*.

3. **Compliment de la Norma ISO/IEC 27001 i ENS:**
   * Separa estrictament els usuaris corporatius, la gestió de xarxa i els serveis exposats a Internet mitjançant tallafocs de nova generació.

---

## 🔍 5. Verificació d'Absència de Solapaments i Rangs Lliures

Totes les subxarxes compten amb màscares `/24` o `/30` estrictament contigües i sense solapaments:

| Subxarxa / VLAN | Xarxa Base | Primera IP Útil | Última IP Útil | Adreça de Difusió (*Broadcast*) | Marge Lliure / Reserva |
|---|---|---|---|---|---|
| *Trànsit R1-FW1* | `192.168.0.0/30` | `192.168.0.1` | `192.168.0.2` | `192.168.0.3` | Totalment assignada (Enllaç punt a punt) |
| *Trànsit R2-FW2* | `192.168.0.4/30` | `192.168.0.5` | `192.168.0.6` | `192.168.0.7` | Totalment assignada (Enllaç punt a punt) |
| *Sincronització HA* | `192.168.0.8/30` | `192.168.0.9` | `192.168.0.10` | `192.168.0.11` | Totalment assignada (Enllaç clúster) |
| *Inter-Core Link* | `192.168.0.12/30` | `192.168.0.13` | `192.168.0.14` | `192.168.0.15` | Totalment assignada (Enllaç switches) |
| *Reserva Trànsit* | `192.168.0.16/28` | `192.168.0.17` | `192.168.0.30` | `192.168.0.31` | 14 IPs disponibles per a nous enllaços |
| **VLAN 10 (Gestió)** | `192.168.10.0/24` | `192.168.10.1` | `192.168.10.254` | `192.168.10.255` | 195 IPs lliures per a nous switches ToR i KVMs |
| **VLAN 20 (DMZ)** | `192.168.20.0/24` | `192.168.20.1` | `192.168.20.254` | `192.168.20.255` | 248 IPs lliures per a nous portals o proxies |
| **VLAN 30 (Interns)** | `192.168.30.0/24` | `192.168.30.1` | `192.168.30.254` | `192.168.30.255` | 244 IPs lliures per a nous nodes de clúster intern |
| **VLAN 40 (Oficina)** | `192.168.40.0/24` | `192.168.40.1` | `192.168.40.254` | `192.168.40.255` | 100 IPs lliures al pool DHCP per a creixement |
| **VLAN 50 (Wi-Fi Corp)** | `192.168.50.0/24` | `192.168.50.1` | `192.168.50.254` | `192.168.50.255` | 220 IPs lliures al pool dinàmic |
| **VLAN 60 (Wi-Fi Visites)**| `192.168.60.0/24` | `192.168.60.1` | `192.168.60.254` | `192.168.60.255` | 240 IPs lliures al pool dinàmic temporal |
| **VLAN 70 (Control IoT)**| `192.168.70.0/24` | `192.168.70.1` | `192.168.70.254` | `192.168.70.255` | 200 IPs lliures per a noves sondes o càmeres |
| **VLAN 101 (Planta 2)** | `192.168.101.0/24`| `192.168.101.1`| `192.168.101.254`| `192.168.101.255`| 170 IPs lliures a la Sala Blanca 1 |
| **VLAN 102 (Planta 3)** | `192.168.102.0/24`| `192.168.102.1`| `192.168.102.254`| `192.168.102.255`| 170 IPs lliures a la Sala Blanca 2 |
| **VLAN 103 (Planta 4)** | `192.168.103.0/24`| `192.168.103.1`| `192.168.103.254`| `192.168.103.255`| 170 IPs lliures a la Sala Blanca 3 |
| **VLAN 104 (Planta 5)** | `192.168.104.0/24`| `192.168.104.1`| `192.168.104.254`| `192.168.104.255`| 170 IPs lliures a la Sala Blanca 4 |

> [!NOTE]
> **Conclusió de la validació:** L'esquema `192.168.[VLAN].X/24` garanteix un disseny sense solapaments, unívoc i fàcilment comprensible, on cada VLAN disposa de la seua pròpia passarel·la al `.1`.
