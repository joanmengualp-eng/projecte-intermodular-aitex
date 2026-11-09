<h1 align="center">🌐 Pla d'Adreçament IP i Noms dels Equips</h1>

<p align="center">
  <img alt="Empresa" src="https://img.shields.io/badge/Aitex_Datacenter-1F3A5F?style=for-the-badge">
  <img alt="Bloc" src="https://img.shields.io/badge/Bloc_2-Disseny_i_Viabilitat-2E7D6B?style=for-the-badge">
  <img alt="Xarxa" src="https://img.shields.io/badge/Subxarxes-VLANs-D97706?style=for-the-badge">
</p>

<p align="center"><em>Planificació de l'adreçament IP privat, segmentació en subxarxes (VLANs), nomenclatura d'equips i separació de dominis de difusió per a Aitex Datacenter.</em></p>

---

## 📐 1. Estructura General de Subxarxes (VLANs)

Per garantir la seguretat, reduir el trànsit de difusió (*broadcast*) i aïllar els entorns crítics, l'adreçament IP privat d'Aitex Datacenter s'organitza sota la xarxa principal **`10.48.0.0/16`** (fent referència a la seu del Carrer País Valencià, 48).

| ID VLAN | Nom de la VLAN | Rang d'Adreces IP | Màscara / Prefix | Passarel·la (Gateway) | Ús i Descripció |
|---|---|---|:---:|:---:|---|
| **VLAN 10** | `VLAN-MGMT` | `10.48.10.0/24` | `255.255.255.0` (/24) | `10.48.10.1` | Gestió d'electrònica de xarxa (Routers, Switches, Firewalls, Punts Wi-Fi). |
| **VLAN 20** | `VLAN-INT-SRV` | `10.48.20.0/24` | `255.255.255.0` (/24) | `10.48.20.1` | 7 Servidors d'ús intern (Active Directory, DNS, Backups, ERP, Monitoratge). |
| **VLAN 30** | `VLAN-STAFF` | `10.48.30.0/24` | `255.255.255.0` (/24) | `10.48.30.1` | 18 Equips d'usuari (PCs de sobretaula i portàtils dels 16 treballadors). |
| **VLAN 100** | `DMZ-P1-CLIENTS` | `10.48.100.0/22` | `255.255.252.0` (/22) | `10.48.100.1` | DMZ Planta 2 (Sales Blanques 1): 80 Servidors de producció B2B. |
| **VLAN 200** | `DMZ-P2-CLIENTS` | `10.48.200.0/22` | `255.255.252.0` (/22) | `10.48.200.1` | DMZ Planta 3 (Sales Blanques 2): 80 Servidors de producció B2B. |
| **VLAN 300** | `DMZ-P3-CLIENTS` | `10.48.300.0/22` | `255.255.252.0` (/22) | `10.48.1.1`* | DMZ Planta 4 (Sales Blanques 3): 80 Servidors de producció B2B (`10.48.1.0/22`). |
| **VLAN 400** | `DMZ-P4-CLIENTS` | `10.48.5.0/22` | `255.255.252.0` (/22) | `10.48.5.1` | DMZ Planta 5 (Sales Blanques 4): 80 Servidors de producció B2B. |

---

## 📋 2. Taula d'Adreçament IP i Nomenclatura d'Equips

### 🛡️ A. Electrònica de Xarxa i Gestió (VLAN 10)

| Nom de l'Equip (Hostname) | Xarxa / VLAN | Adreça IP | Prefix | Passarel·la | Descripció |
|---|---|:---:|:---:|:---:|---|
| `GW-EDGE-01` | `VLAN-MGMT` | `10.48.10.1` | /24 | N/A | Router de Vora Principal (Gateway de Gestió) |
| `GW-EDGE-02` | `VLAN-MGMT` | `10.48.10.2` | /24 | N/A | Router de Vora Secundari (Redundant) |
| `FW-PERIM-01` | `VLAN-MGMT` | `10.48.10.3` | /24 | `10.48.10.1` | Tallafocs Perimetral i Anti-DDoS Principal |
| `FW-PERIM-02` | `VLAN-MGMT` | `10.48.10.4` | /24 | `10.48.10.1` | Tallafocs Perimetral i Anti-DDoS Secundari |
| `SW-CORE-01` | `VLAN-MGMT` | `10.48.10.5` | /24 | `10.48.10.1` | Switch Core de Fibra Principal |
| `SW-CORE-02` | `VLAN-MGMT` | `10.48.10.6` | /24 | `10.48.10.1` | Switch Core de Fibra Secundari |
| `AP-OFFICE-01..02` | `VLAN-MGMT` | `10.48.10.11..12` | /24 | `10.48.10.1` | Punts d'Accés Wi-Fi Oficina (Planta 1) |
| `AP-DC-P1..P4` | `VLAN-MGMT` | `10.48.10.13..16` | /24 | `10.48.10.1` | Punts d'Accés Wi-Fi Sales Blanques (Plantes 2 a 5) |

---

### ⚙️ B. Servidors d'Ús Intern i Gestió (VLAN 20)

| Nom de l'Equip (Hostname) | Xarxa / VLAN | Adreça IP | Prefix | Passarel·la | Funció |
|---|---|:---:|:---:|:---:|---|
| `SRV-INT-DC01` | `VLAN-INT-SRV` | `10.48.20.10` | /24 | `10.48.20.1` | Control de Domini i DNS Principal |
| `SRV-INT-DC02` | `VLAN-INT-SRV` | `10.48.20.11` | /24 | `10.48.20.1` | Control de Domini i DNS Secundari |
| `SRV-INT-MON01` | `VLAN-INT-SRV` | `10.48.20.20` | /24 | `10.48.20.1` | Servidor de Monitoratge de Servidors i Xarxa |
| `SRV-INT-MON02` | `VLAN-INT-SRV` | `10.48.20.21` | /24 | `10.48.20.1` | Servidor de Seguretat SIEM i Anàlisi de Logs |
| `SRV-INT-ERP01` | `VLAN-INT-SRV` | `10.48.20.30` | /24 | `10.48.20.1` | Portal Web de Clients B2B, Facturació i Tíquets |
| `SRV-INT-BAK01` | `VLAN-INT-SRV` | `10.48.20.40` | /24 | `10.48.20.1` | Emmagatzematge i Còpies de Seguretat Internes (NAS 1) |
| `SRV-INT-BAK02` | `VLAN-INT-SRV` | `10.48.20.41` | /24 | `10.48.20.1` | Emmagatzematge i Còpies de Seguretat Internes (NAS 2) |

---

### 💻 C. Equips d'Usuari i Treballadors (VLAN 30)

| Departament | Rango Hostnames | Adreces IP | Prefix | Passarel·la | Nre. Equips |
|---|---|:---:|:---:|:---:|:---:|
| 👔 **Administració, Comercial i Dir.** | `PC-ADM-01..03` | `10.48.30.10 .. 10.48.30.12` | /24 | `10.48.30.1` | 3 PCs / Portàtils |
| 🛠️ **Sistemes i Suport Tècnic** | `PC-SYS-01..06`<br>`LAP-SYS-01..02` | `10.48.30.20 .. 10.48.30.25`<br>`10.48.30.26 .. 10.48.30.27` | /24 | `10.48.30.1` | 6 PCs Sobretaula<br>2 Portàtils |
| 🔒 **Xarxes i Ciberseguretat (NOC)** | `PC-NOC-01..04` | `10.48.30.30 .. 10.48.30.33` | /24 | `10.48.30.1` | 4 PCs Multipantalla |
| ⚡ **Manteniment i Infraestructura** | `LAP-MNT-01..03` | `10.48.30.40 .. 10.48.30.42` | /24 | `10.48.30.1` | 3 Portàtils Robustos |

---

### 🗄️ D. Servidors de Producció en DMZ (VLANs 100, 200, 300, 400)

| Planta / Ubicació | Rang de Servidors | Rang IP Assignat | Prefix | Passarel·la | Nre. Servidors |
|---|---|:---:|:---:|:---:|:---:|
| 🏢 **Planta 2 (Sala Blanques 1)** | `SRV-NODE-001 .. 080` | `10.48.100.10 .. 10.48.100.89` | /22 | `10.48.100.1` | 80 Servidors B2B |
| 🏢 **Planta 3 (Sala Blanques 2)** | `SRV-NODE-081 .. 160` | `10.48.200.10 .. 10.48.200.89` | /22 | `10.48.200.1` | 80 Servidors B2B |
| 🏢 **Planta 4 (Sala Blanques 3)** | `SRV-NODE-161 .. 240` | `10.48.1.10 .. 10.48.1.89` | /22 | `10.48.1.1` | 80 Servidors B2B |
| 🏢 **Planta 5 (Sala Blanques 4)** | `SRV-NODE-241 .. 320` | `10.48.5.10 .. 10.48.5.89` | /22 | `10.48.5.1` | 80 Servidors B2B |

---

## 🔒 3. Justificació de la Separació entre Xarxa Interna i DMZ

La separació física i lògica entre la **Xarxa Interna** i la **DMZ** és un requisit imprescindible per complir amb l'arquitectura d'un Centre de Processament de Dades d'alta seguretat:

1. **Aïllament d'Atacs Externs:** Els 320 servidors de producció allotgen dades i aplicacions web de clients B2B accessibles des d'Internet. Al situar-los en la DMZ, si un d'aquests servidors fóra compromès per un atac extern, el tallafocs perimetral (`FW-PERIM-01/02`) impedeix que l'atacant puga saltar (*moviment lateral*) a la xarxa interna de gestió (`VLAN-INT-SRV`) ni accedir a les dades d'administració de l'empresa.
2. **Control de Polítiques d'Accés:** La xarxa d'oficina (`VLAN-STAFF`) té accés restringit als hipervisors i servidors interns mitjançant autenticació de doble factor (2FA) i connexió xifrada (SSH/HTTPS).
3. **Absència de Solapaments:** L'estructura d'adreçament utilitza blocs de màscares de subxarxa ben definits (`/24` per a gestió i oficina; `/22` per a la capacitat d'ampliació dels servidors de producció), garantint que cap adreça IP es trobe duplicada o en conflicte entre diferents VLANs.
