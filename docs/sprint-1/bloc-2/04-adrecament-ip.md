<h1 align="center">🗺️ Tasca 4. Pla d'adreçament IP i noms dels equips</h1>

<p align="center">
  <img alt="Fase" src="https://img.shields.io/badge/Fase-Disseny_T%C3%A8cnic-D97706?style=for-the-badge">
  <img alt="Projecte" src="https://img.shields.io/badge/Projecte-Aitex_Datacenter-2E7D6B?style=for-the-badge">
  <img alt="CE" src="https://img.shields.io/badge/Criteris_Avaluaci%C3%B3-RA2.e_·_RA2.i-2E7D6B?style=for-the-badge">
</p>

Aquest document defineix el pla exhaustiu d'adreçament IP privat, la segmentació lògica en xarxes virtuals (**VLANs**), el criteri de nomenclatura normalitzat (*hostname/FQDN*) dels equips i la justificació de seguretat entre la xarxa interna i la **DMZ** per a **Aitex Datacenter**.

---

## 📐 1. Esquema General de Subxarxes i VLANs

S'ha adoptat l'espai d'adreçament privat **RFC 1918** de classe A reservat **`10.20.0.0/16`**, el qual ofereix un espai de fins a 65.534 adreces dividit jeràrquicament en blocs `/24` (254 hosts per subxarxa) per a les diferents zones funcionals, més blocs `/30` per als enllaços d'interconnexió punt a punt.

| VLAN ID | Nom de la Subxarxa | Xarxa (CIDR) | Màscara de Subxarxa | Passarel·la (Gateway) | Rang d'adreces útils | Tipus d'assignació | Funció / Zona |
|:---:|---|---|---|---|---|:---:|---|
| **—** | *Trànsit R1 - FW1* | `10.20.0.0/30` | `255.255.255.252` | `10.20.0.1` | `10.20.0.1 - 10.20.0.2` | Estàtica | Enllaç punt a punt Router 1 i Firewall 1 |
| **—** | *Trànsit R2 - FW2* | `10.20.0.4/30` | `255.255.255.252` | `10.20.0.5` | `10.20.0.5 - 10.20.0.6` | Estàtica | Enllaç punt a punt Router 2 i Firewall 2 |
| **—** | *Sincronització FW HA* | `10.20.0.8/30` | `255.255.255.252` | `—` | `10.20.0.9 - 10.20.0.10` | Estàtica | Heartbeat i estat del clúster de tallafocs |
| **—** | *Enllaç Inter-Core (LACP)*| `10.20.0.12/30` | `255.255.255.252` | `—` | `10.20.0.13 - 10.20.0.14` | Estàtica | Trànsit entre Switches Core 1 i 2 |
| **10** | **VLAN_GESTIO_OOBM** | `10.20.10.0/24` | `255.255.255.0` | `10.20.10.1` | `10.20.10.1 - 10.20.10.254` | Estàtica | Xarxa fora de banda: IPMI, iLO, KVM sobre IP i gestió de switches |
| **20** | **VLAN_DMZ_PUBLICA** | `10.20.20.0/24` | `255.255.255.0` | `10.20.20.1` | `10.20.20.1 - 10.20.20.254` | Estàtica | Serveis amb accés públic des d'Internet (Web/ERP, DNS, Mail) |
| **30** | **VLAN_SRV_INTERNS** | `10.20.30.0/24` | `255.255.255.0` | `10.20.30.1` | `10.20.30.1 - 10.20.30.254` | Estàtica | Controladors de Domini (AD), Backups corporatius, SIEM/Syslog |
| **40** | **VLAN_OFICINA_USUARIS**| `10.20.40.0/24` | `255.255.255.0` | `10.20.40.1` | `10.20.40.1 - 10.20.40.254` | DHCP (`.50 - .200`) | PCs sobretaula d'Administració, Direcció, Suport i SOC |
| **50** | **VLAN_WIFI_CORP** | `10.20.50.0/24` | `255.255.255.0` | `10.20.50.1` | `10.20.50.1 - 10.20.50.254` | DHCP (`.20 - .250`) | Portàtils tècnics corporatius (WPA3-Enterprise / 802.1X) |
| **60** | **VLAN_WIFI_VISITES** | `10.20.60.0/24` | `255.255.255.0` | `10.20.60.1` | `10.20.60.1 - 10.20.60.254` | DHCP (`.10 - .250`) | Accés a Internet exclusiu per a visites amb portal captiu |
| **70** | **VLAN_IOT_BMS_CONTROL**| `10.20.70.0/24` | `255.255.255.0` | `10.20.70.1` | `10.20.70.1 - 10.20.70.254` | Estàtica / DHCP | Sensors de temperatura, humitat, SAIs, Videowall NOC i càmeres CCTV |
| **101** | **VLAN_PROD_PLANTA2** | `10.20.101.0/24` | `255.255.255.0` | `10.20.101.1` | `10.20.101.1 - 10.20.101.254` | Estàtica | Sala Blanca 1: 80 servidors de producció clients B2B |
| **102** | **VLAN_PROD_PLANTA3** | `10.20.102.0/24` | `255.255.255.0` | `10.20.102.1` | `10.20.102.1 - 10.20.102.254` | Estàtica | Sala Blanca 2: 80 servidors de producció clients B2B |
| **103** | **VLAN_PROD_PLANTA4** | `10.20.103.0/24` | `255.255.255.0` | `10.20.103.1` | `10.20.103.1 - 10.20.103.254` | Estàtica | Sala Blanca 3: 80 servidors de producció clients B2B |
| **104** | **VLAN_PROD_PLANTA5** | `10.20.104.0/24` | `255.255.255.0` | `10.20.104.1` | `10.20.104.1 - 10.20.104.254` | Estàtica | Sala Blanca 4: 80 servidors de producció clients B2B |

---

## 🏷️ 2. Estàndard de Nomenclatura d'Equips (*Hostname / FQDN*)

Tots els equips de la xarxa segueixen un patró de nomenclatura jeràrquic i unívoc:

$$\text{Codi Seu} - \text{Tipus Equip} - \text{Funció / Zona} - \text{Número Seqüencial}$$

* **Codi de Seu:** `ALC` (Seu Central d'Alcoi).
* **Tipus de Dispositiu:**
  * `RTR`: Encaminador (Router).
  * `FW`: Tallafocs de Nova Generació (Firewall).
  * `SWC`: Switch de Nucli (Core Layer 3).
  * `SWA`: Switch d'Accés / Top-of-Rack (ToR Layer 2+).
  * `AP`: Punt d'Accés Wi-Fi (Access Point).
  * `SRV`: Servidor físic o virtualitzat.
  * `PC`: Estació de treball fixa de sobretaula.
  * `LT`: Ordinador portàtil corporatiu (*Laptop*).
  * `PRN`: Impressora en xarxa.
  * `IOT`: Dispositiu IoT, SAI o control ambiental.
* **Domini intern:** `intra.aitexdatacenter.com`

---

## 💻 3. Taula d'Adreçament d'Equips i Dispositius de Xarxa

A continuació es detalla l'assignació IP prevista per als principals components físics i lògics de la infraestructura:

### A. Electrònica de Xarxa, Seguretat i Enllaços de Trànsit

| Nom de l'Equip (*Hostname*) | Xarxa / VLAN | Adreça IP | Màscara / Prefix | Passarel·la (*Gateway*) | Descripció i Funció |
|---|---|:---:|:---:|:---:|---|
| `ALC-RTR-EDGE01` | Trànsit ISP 1 | `203.0.113.2` | `255.255.255.252` (`/30`) | `203.0.113.1` | Router de Vora 1 (BGP Primari, Fibra ISP 1) |
| `ALC-RTR-EDGE01` (LAN) | Trànsit R1-FW1 | `10.20.0.1` | `255.255.255.252` (`/30`) | `—` | Pota interna cap al Firewall 1 |
| `ALC-RTR-EDGE02` | Trànsit ISP 2 | `198.51.100.2` | `255.255.255.252` (`/30`) | `198.51.100.1` | Router de Vora 2 (BGP Secundari, Fibra ISP 2) |
| `ALC-RTR-EDGE02` (LAN) | Trànsit R2-FW2 | `10.20.0.5` | `255.255.255.252` (`/30`) | `—` | Pota interna cap al Firewall 2 |
| `ALC-FW-PERIM01` (WAN) | Trànsit R1-FW1 | `10.20.0.2` | `255.255.255.252` (`/30`) | `10.20.0.1` | Firewall Primari Actiu (Interfície WAN) |
| `ALC-FW-PERIM02` (WAN) | Trànsit R2-FW2 | `10.20.0.6` | `255.255.255.252` (`/30`) | `10.20.0.5` | Firewall Secundari Passiu (Interfície WAN) |
| `ALC-FW-PERIM01` (HA Sync)| Sincronització | `10.20.0.9` | `255.255.255.252` (`/30`) | `—` | Port de sincronització d'estat Clúster HA |
| `ALC-FW-PERIM02` (HA Sync)| Sincronització | `10.20.0.10` | `255.255.255.252` (`/30`) | `—` | Port de sincronització d'estat Clúster HA |
| `ALC-FW-VIP-DMZ` | VLAN 20 (DMZ) | `10.20.20.1` | `255.255.255.0` (`/24`) | `—` | IP Virtual Clúster Firewall per a la DMZ (Gateway) |
| `ALC-SWC-CORE01` | VLAN 10 (Gestió) | `10.20.10.2` | `255.255.255.0` (`/24`) | `10.20.10.1` | Switch Core 1 L3 (Gestió i Inter-VLAN Routing) |
| `ALC-SWC-CORE02` | VLAN 10 (Gestió) | `10.20.10.3` | `255.255.255.0` (`/24`) | `10.20.10.1` | Switch Core 2 L3 (Stacking/VSS amb Core 1) |
| `ALC-SWA-OFI01` | VLAN 10 (Gestió) | `10.20.10.4` | `255.255.255.0` (`/24`) | `10.20.10.1` | Switch PoE Planta 1 (Oficina i Punts d'Accés) |
| `ALC-SWA-OFI02` | VLAN 10 (Gestió) | `10.20.10.5` | `255.255.255.0` (`/24`) | `10.20.10.1` | Switch PoE Planta 1 Redundant (Telèfons IP) |
| `ALC-SWA-TOR01` a `08` | VLAN 10 (Gestió) | `10.20.10.11` - `.18` | `255.255.255.0` (`/24`) | `10.20.10.1` | 8 Switches ToR Planta 2 (Sala Blanca 1) |
| `ALC-SWA-TOR09` a `16` | VLAN 10 (Gestió) | `10.20.10.19` - `.26` | `255.255.255.0` (`/24`) | `10.20.10.1` | 8 Switches ToR Planta 3 (Sala Blanca 2) |
| `ALC-SWA-TOR17` a `24` | VLAN 10 (Gestió) | `10.20.10.27` - `.34` | `255.255.255.0` (`/24`) | `10.20.10.1` | 8 Switches ToR Planta 4 (Sala Blanca 3) |
| `ALC-SWA-TOR25` a `32` | VLAN 10 (Gestió) | `10.20.10.35` - `.42` | `255.255.255.0` (`/24`) | `10.20.10.1` | 8 Switches ToR Planta 5 (Sala Blanca 4) |
| `ALC-AP-OFI01` - `02` | VLAN 10 (Gestió) | `10.20.10.51` - `.52` | `255.255.255.0` (`/24`) | `10.20.10.1` | Punts d'Accés Wi-Fi Planta 1 (Oficines) |
| `ALC-AP-SB01` a `04` | VLAN 10 (Gestió) | `10.20.10.53` - `.56` | `255.255.255.0` (`/24`) | `10.20.10.1` | Punts d'Accés Wi-Fi Sales Blanques (Plantes 2 a 5) |

---

### B. Serveis en DMZ i Servidors Interns

| Nom de l'Equip (*Hostname*) | Xarxa / VLAN | Adreça IP | Màscara / Prefix | Passarel·la (*Gateway*) | Descripció i Serveis |
|---|---|:---:|:---:|:---:|---|
| `ALC-SRV-WEB01` | VLAN 20 (DMZ) | `10.20.20.10` | `255.255.255.0` (`/24`) | `10.20.20.1` | Portal Web corporatiu i ERP d'Autoservei B2B (HTTPS) |
| `ALC-SRV-MAIL01` | VLAN 20 (DMZ) | `10.20.20.20` | `255.255.255.0` (`/24`) | `10.20.20.1` | Relay de Correu / Passarel·la Anti-Spam i Anti-Malware (SMTP) |
| `ALC-SRV-DNSEXT01` | VLAN 20 (DMZ) | `10.20.20.30` | `255.255.255.0` (`/24`) | `10.20.20.1` | Servidor DNS Autoritatiu Públic per al domini comercial |
| `ALC-SRV-DC01` | VLAN 30 (Interns)| `10.20.30.10` | `255.255.255.0` (`/24`) | `10.20.30.1` | Controlador de Domini Primari (Active Directory, DNS/DHCP intern) |
| `ALC-SRV-DC02` | VLAN 30 (Interns)| `10.20.30.11` | `255.255.255.0` (`/24`) | `10.20.30.1` | Controlador de Domini Secundari (Rèplica i Alta Disponibilitat) |
| `ALC-SRV-MON01` | VLAN 30 (Interns)| `10.20.30.20` | `255.255.255.0` (`/24`) | `10.20.30.1` | Servidor SIEM i Monitoratge Zabbix / Grafana (Syslog/SNMP) |
| `ALC-SRV-BMS01` | VLAN 30 (Interns)| `10.20.30.25` | `255.255.255.0` (`/24`) | `10.20.30.1` | Plataforma BMS (Building Management) per a telemetria ambiental |
| `ALC-SRV-BKP01` | VLAN 30 (Interns)| `10.20.30.30` | `255.255.255.0` (`/24`) | `10.20.30.1` | Servidor de Còpies de Seguretat Primari (Veeam / Proxmox Backup) |
| `ALC-SRV-BKP02` | VLAN 30 (Interns)| `10.20.30.31` | `255.255.255.0` (`/24`) | `10.20.30.1` | Cabina d'Emmagatzematge i rèplica de snapshots externs |
| `ALC-SRV-FILE01` | VLAN 30 (Interns)| `10.20.30.40` | `255.255.255.0` (`/24`) | `10.20.30.1` | Servidor de Fitxers intern per a contractes, ISOs i administració |

---

### C. Equips Clients de la Plantilla, Perifèrics i Rangs de Producció

| Nom de l'Equip (*Hostname*) | Xarxa / VLAN | Adreça IP | Màscara / Prefix | Passarel·la (*Gateway*) | Descripció del lloc de treball |
|---|---|:---:|:---:|:---:|---|
| `ALC-PC-DIR01` | VLAN 40 (Oficina) | `10.20.40.10` | `255.255.255.0` (`/24`) | `10.20.40.1` | Lloc de Direcció General (CEO) |
| `ALC-PC-ADM01` | VLAN 40 (Oficina) | `10.20.40.11` | `255.255.255.0` (`/24`) | `10.20.40.1` | Responsable d'Administració, RRHH i Finances |
| `ALC-PC-COM01` | VLAN 40 (Oficina) | `10.20.40.12` | `255.255.255.0` (`/24`) | `10.20.40.1` | Comercial B2B i Màrqueting |
| `ALC-PC-SYS01` a `06` | VLAN 40 (Oficina) | `10.20.40.21` - `.26` | `255.255.255.0` (`/24`) | `10.20.40.1` | 6 Estacions de Sistemes i Suport Tècnic N1/N2 (Doble monitor) |
| `ALC-PC-SOC01` a `04` | VLAN 40 (Oficina) | `10.20.40.31` - `.34` | `255.255.255.0` (`/24`) | `10.20.40.1` | 4 Estacions de Treball Xarxes i Ciberseguretat NOC/SOC |
| `ALC-LT-SYS01` - `02` | VLAN 50 (Wi-Fi Corp)| `10.20.50.21` - `.22` (DHCP) | `255.255.255.0` (`/24`) | `10.20.50.1` | 2 Portàtils Tècnics de Sistemes (Mobilitat i diagnòstic) |
| `ALC-LT-INF01` a `03` | VLAN 50 (Wi-Fi Corp)| `10.20.50.31` - `.33` (DHCP) | `255.255.255.0` (`/24`) | `10.20.50.1` | 3 Portàtils Robustos (*Rugged*) de Manteniment Físic |
| `ALC-PRN-LASER01` | VLAN 40 (Oficina) | `10.20.40.50` | `255.255.255.0` (`/24`) | `10.20.40.1` | Impressora multifunció làser departamental |
| `ALC-PRN-LBL01` - `02`| VLAN 40 (Oficina) | `10.20.40.51` - `.52` | `255.255.255.0` (`/24`) | `10.20.40.1` | 2 Impressores tèrmiques d'etiquetatge per a racks/cables |
| `ALC-IOT-VW01` | VLAN 70 (IoT/BMS) | `10.20.70.10` | `255.255.255.0` (`/24`) | `10.20.70.1` | Controlador gràfic del Videowall (4 pantalles NOC 55") |
| `ALC-PROD-P2-01` a `80`| VLAN 101 (Planta 2) | `10.20.101.10` - `.89` | `255.255.255.0` (`/24`) | `10.20.101.1` | 80 Servidors Físics Sala Blanca 1 (*Half/Full-Server*) |
| `ALC-PROD-P3-01` a `80`| VLAN 102 (Planta 3) | `10.20.102.10` - `.89` | `255.255.255.0` (`/24`) | `10.20.102.1` | 80 Servidors Físics Sala Blanca 2 (*Half/Full-Server*) |
| `ALC-PROD-P4-01` a `80`| VLAN 103 (Planta 4) | `10.20.103.10` - `.89` | `255.255.255.0` (`/24`) | `10.20.103.1` | 80 Servidors Físics Sala Blanca 3 (*Half/Full-Server*) |
| `ALC-PROD-P5-01` a `80`| VLAN 104 (Planta 5) | `10.20.104.10` - `.89` | `255.255.255.0` (`/24`) | `10.20.104.1` | 80 Servidors Físics Sala Blanca 4 (*Half/Full-Server*) |

---

## 🛡️ 4. Justificació de la Separació entre Xarxa Interna i DMZ

La creació d'una **Zona Desmilitaritzada (DMZ - VLAN 20)** aïllada de la **Xarxa Interna corporativa (VLAN 30 i 40)** i de la **Xarxa de Gestió (VLAN 10)** és un requisit innegociable de seguretat fonamentat en els següents principis:

```
               [ INTERNET (Públic) ]
                         │
                         ▼
             [ CLÚSTER FIREWALL (NGFW) ]
             ┌───────────┴───────────┐
             │ (Filtratge Estricte)  │
             ▼                       ▼
    [ ZONA DMZ (VLAN 20) ]    [ XARXA INTERNA (VLAN 30/40) ]
    • Portal Web / ERP        • Directori Actiu (AD/LDAP)
    • Relay de Correu         • Servidor de Còpies (Backups)
    • DNS Públic              • PCs de Treball dels Empleats
             │                       ▲
             └───[ BLOQUEJAT ]───────┘ (Default Deny)
```

1. **Principi de Mínima Confiança (*Zero Trust*) i Aïllament de Breixes:**
   * Els serveis web (`ALC-SRV-WEB01`) han d'atendre peticions públiques des de qualsevol IP del món pels ports TCP 80 i 443. Si un atacant troba una vulnerabilitat a l'aplicació web (ex. injecció SQL o execució remota de codi), queda confinat a la subxarxa `10.20.20.0/24`.
   * **Regla Tallafocs Fonamental:** El Firewall perimetral té configurada una política de **Denegació per Defecte (*Default Deny*)**: cap equip situat a la DMZ pot iniciar sessions ni obrir connexions cap a la xarxa interna (`10.20.30.0/24` o `10.20.40.0/24`).

2. **Protecció del Nucli Operatiu i Dades Confidencials:**
   * La xarxa interna allotja el Directori Actiu, les bases de dades de facturació, les nòmines i les còpies de seguretat (Veeam). Si la DMZ i la xarxa interna compartiren el mateix domini de difusió, qualsevol malware o atac de moviment lateral (*Pass-the-Hash*, *ransomware*) infectaria la infraestructura central.

3. **Compliment Normatiu Internacional (ISO/IEC 27001 i RGPD):**
   * L'estàndard ISO 27001 (control A.13.1 sobre gestió de la seguretat en les xarxes) exigeix separar els serveis d'informació, els usuaris i els sistemes de comunicacions en xarxes protegides mitjançant passarel·les de seguretat dedicades.

---

## 🔍 5. Verificació d'Absència de Solapaments i Adreces Duplicades

Per garantir matemàticament que el pla d'adreçament no conté **cap solapament d'adreces (*overlapping*)** ni col·lisions d'IPs, s'ha realitzat l'anàlisi de rangs contigus:

| Subxarxa / VLAN | Xarxa Base | Primera IP Útil | Última IP Útil | Adreça de Difusió (*Broadcast*) | Marge Lliure / Reserva |
|---|---|---|---|---|---|
| *Trànsit R1-FW1* | `10.20.0.0/30` | `10.20.0.1` | `10.20.0.2` | `10.20.0.3` | Totalment assignada (Enllaç punt a punt) |
| *Trànsit R2-FW2* | `10.20.0.4/30` | `10.20.0.5` | `10.20.0.6` | `10.20.0.7` | Totalment assignada (Enllaç punt a punt) |
| *Sincronització HA* | `10.20.0.8/30` | `10.20.0.9` | `10.20.0.10` | `10.20.0.11` | Totalment assignada (Enllaç clúster) |
| *Inter-Core Link* | `10.20.0.12/30` | `10.20.0.13` | `10.20.0.14` | `10.20.0.15` | Totalment assignada (Enllaç switches) |
| *Reserva Trànsit* | `10.20.0.16/28` | `10.20.0.17` | `10.20.0.30` | `10.20.0.31` | 14 IPs disponibles per a futurs enllaços |
| **VLAN 10 (Gestió)** | `10.20.10.0/24` | `10.20.10.1` | `10.20.10.254` | `10.20.10.255` | 195 IPs lliures per a nous switches ToR i KVMs |
| **VLAN 20 (DMZ)** | `10.20.20.0/24` | `10.20.20.1` | `10.20.20.254` | `10.20.20.255` | 248 IPs lliures per a balancejadors o nous portals |
| **VLAN 30 (Interns)** | `10.20.30.0/24` | `10.20.30.1` | `10.20.30.254` | `10.20.30.255` | 244 IPs lliures per a nous nodes de clúster intern |
| **VLAN 40 (Oficina)** | `10.20.40.0/24` | `10.20.40.1` | `10.20.40.254` | `10.20.40.255` | 100 IPs lliures al pool DHCP per a creixement |
| **VLAN 50 (Wi-Fi Corp)** | `10.20.50.0/24` | `10.20.50.1` | `10.20.50.254` | `10.20.50.255` | 220 IPs lliures al pool dinàmic |
| **VLAN 60 (Wi-Fi Visites)**| `10.20.60.0/24` | `10.20.60.1` | `10.20.60.254` | `10.20.60.255` | 240 IPs lliures al pool dinàmic temporal |
| **VLAN 70 (IoT/BMS)** | `10.20.70.0/24` | `10.20.70.1` | `10.20.70.254` | `10.20.70.255` | 200 IPs lliures per a noves sondes o càmeres |
| **VLAN 101 (Planta 2)** | `10.20.101.0/24`| `10.20.101.1`| `10.20.101.254`| `10.20.101.255`| 170 IPs lliures a la Sala Blanca 1 |
| **VLAN 102 (Planta 3)** | `10.20.102.0/24`| `10.20.102.1`| `10.20.102.254`| `10.20.102.255`| 170 IPs lliures a la Sala Blanca 2 |
| **VLAN 103 (Planta 4)** | `10.20.103.0/24`| `10.20.103.1`| `10.20.103.254`| `10.20.103.255`| 170 IPs lliures a la Sala Blanca 3 |
| **VLAN 104 (Planta 5)** | `10.20.104.0/24`| `10.20.104.1`| `10.20.104.254`| `10.20.104.255`| 170 IPs lliures a la Sala Blanca 4 |

> [!NOTE]
> **Conclusió de la validació:** Totes les màscares `/24` i `/30` respecten de manera estricta els límits binaris naturals del tercer i quart octet. No existeix cap solapament de rangs i cada equip compta amb una passarel·la unívoca assignada a la seua respectiva interfície VLAN del switch *Core* L3 o del clúster de tallafocs.
