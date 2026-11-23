<h1 align="center">🖥️ Relació de maquinari i programari</h1>

<p align="center">
  <img alt="Empresa" src="https://img.shields.io/badge/Empresa-Aitex_Datacenter-1F3A5F?style=for-the-badge">
  <img alt="Plantilla" src="https://img.shields.io/badge/Plantilla-16_persones-2E7D6B?style=for-the-badge">
  <img alt="Equips d'usuari" src="https://img.shields.io/badge/Equips_usuari-18-3B7DD8?style=for-the-badge">
  <img alt="Servidors" src="https://img.shields.io/badge/Servidors-327-D97706?style=for-the-badge">
</p>

Aquests són els principals recursos d'**Aitex Datacenter** necessaris per mantenir la infraestructura del Centre de Processament de Dades (CPD). Els recursos transversals apareixen com a «Tots els departaments».

> [!TIP]
> **Llegenda:** 🧩 programari · ☁️ servei al núvol · la resta d'icones indiquen maquinari físic.

---

## 📋 Taula de recursos (Maquinari i Programari)

| Recurs | Departament / Àrea | Quantitat | Funció principal |
|---|---|:---:|---|
| 💻 Ordinador d'ofimàtica / portàtil | Administració, Comercial i Dir. | 3 | Gestió comercial B2B, comptabilitat i direcció |
| 🖨️ Impressora multifunció làser | Administració, Comercial i Dir. | 1 | Imprimir contractes B2B i documentació legal |
| 🧩 Programari ERP, CRM i Facturació | Administració, Comercial i Dir. | 1 instància (Odoo Comm.) | Gestió de facturació *Half/Full-Server*, clients B2B i tiquets (Open Source, accés web) |
| 🖥️ Estació de treball d'alt rendiment | Sistemes i Suport Tècnic | 6 | Administració de sistemes operatius i hipervisors (doble monitor 27") |
| 💻 Ordinador portàtil tècnic | Sistemes i Suport Tècnic | 2 | Mobilitat per les 4 plantes de sales blanques per diagnòstic directe |
| 🗄️ Servidors Rack 1U de Producció | Sistemes i Suport Tècnic | 320 | `Dell PowerEdge R360` per allotjar els serveis dels clients (*Half/Full-Server*) |
| 🗄️ Servidors Rack 2U d'ús intern | Sistemes i Suport Tècnic | 7 | `Dell PowerEdge R750xs` (AD/DNS, SIEM, BMS, portal web/ERP i backups) |
| 🧩 Hipervisor de virtualització | Sistemes i Suport Tècnic | 320 sockets | **Proxmox VE** (Subscripció Community, 1 socket per servidor R360) |
| 🧩 SO de Servidor i Còpies de Seguretat | Sistemes i Suport Tècnic | 2 Win. + 5 Deb. + 1 Veeam | 2× Windows Server 2022 Datacenter (AD), 5× Debian 12 i 1 suite Veeam Backup |
| 🖥️ Estació de treball avançada (SOC) | Xarxes i Ciberseguretat | 4 | Estacions multipantalla per a l'anàlisi de trànsit i logs |
| 📺 Videowall (Pantalles de 55") | Xarxes i Ciberseguretat | 4 | `Samsung VH55R-R` per al monitoratge visual permanent 24/7 a la sala NOC |
| 🌍 Routers de vora d'alta capacitat | Xarxes i Ciberseguretat | 2 | `MikroTik CCR2216-1G-12XS-2XQ` (BGP redundant amb dos ISPs de fibra 10G) |
| 🛡️ Tallafocs físics NGFW (Clúster HA) | Xarxes i Ciberseguretat | 2 (+2 subscr.) | `Fortinet FortiGate 400F` amb subscripció FortiGuard UTP + FortiCare 24×7 |
| ⚡ Switches *Core* i d'Accés | Xarxes i Ciberseguretat | 36 | 2 Core `FS S5860-20SQ` + 32 ToR `FS S3900-48T4S` + 2 PoE `Cisco CBS350-48P-4G` |
| 📶 Punts d'accés Wi-Fi 6 | Xarxes i Ciberseguretat | 6 | `Ubiquiti UniFi U6 Pro` (cobertura corporativa i visites a les 5 plantes) |
| 🧩 Programari SIEM i Monitoratge xarxa | Xarxes i Ciberseguretat | 1 plataforma (Open Source) | **Zabbix + Grafana + Wazuh** (IDS, telemetria SNMP i alertes en temps real) |
| 💻 Ordinador portàtil robust (*rugged*) | Manteniment i Infraestructura | 3 | `Dell Latitude 5540` per a supervisió física a les 4 sales blanques |
| 🏷️ Impressora tèrmica d'etiquetes | Manteniment i Infraestructura | 2 | `Brother PT-E550W` per etiquetar servidors, racks i cablatge (TIA-606-B) |
| 🔋 SAIs Trifàsics N+1 i Grup Electrògen | Manteniment i Infraestructura | 3 SAIs + 1 Gen. | 3× `Schneider Galaxy VS 80 kW` (N+1) + 1× Grup Electrògen Dièsel 250 kVA |
| ❄️ Climatització de precisió (CRAC N+1) | Manteniment i Infraestructura | 8 unitats | 8× `Schneider Uniflair 35–40 kW` (2 unitats N+1 per cada sala blanca) |
| 🗄️ Armaris Rack 42U, PDUs A+B i KVMs | Manteniment i Infraestructura | 18 / 36 / 8 | 18 racks `Rittal TS IT`, 36 PDUs intel·ligents (A+B) i 8 consoles KVM `Aten CL5708IM` |
| 🧩 Programari BMS (Building Mngmt.) | Manteniment i Infraestructura | 1 plataforma (Open Source) | Integració de sondes tèrmiques, PDUs i SAIs sobre Zabbix/Grafana BMS |
| ☎️ Telèfon IP amb auriculars | Tots els departaments | 16 | `Yealink T43U` (PoE) per a comunicació interna i suport tècnic |
| 🧩 Sistema operatiu d'ordinador | Tots els departaments | 18 llicències | **Windows 11 Pro** (13 PCs + 5 portàtils, amb dual boot Ubuntu LTS en tècnics) |
| 🧩 Programari ofimàtic i correu | Tots els departaments | 16 usuaris (Open Source) | **LibreOffice** i **Thunderbird** (0 € en llicències privatives) |
| 🧩 Antivirus / EDR corporatiu | Tots els departaments | 18 llicències | **Bitdefender GravityZone Business** (protecció dels 18 equips d'usuari) |

---

## 📝 Observacions

> [!NOTE]
> - En el **maquinari**, la quantitat reflecteix el nombre d'unitats físiques. En el **programari**, indica el nombre de llicències, subscripcions o nodes administrats.
> - Hi ha **18 equips d'usuari** per als 16 treballadors: 13 ordinadors de sobretaula/estacions fixes i 5 portàtils compartits per a les guàrdies i el moviment físic per les 5 plantes del recinte.
> - La infraestructura central es compon de **327 servidors físics** (320 per a clients B2B i 7 d'ús intern i monitoratge).

> [!IMPORTANT]
> Aquesta relació identifica els elements requerits a nivell general. L'arquitectura de xarxa detallada, adreçament IP i els diagrames lògics es desenvoluparan en el **Bloc 2**.
