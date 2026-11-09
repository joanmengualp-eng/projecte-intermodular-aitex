<h1 align="center">📦 Tasca 5. Selecció de recursos materials i humans</h1>

<p align="center">
  <img alt="Fase" src="https://img.shields.io/badge/Fase-Planificaci%C3%B3-1F3A5F?style=for-the-badge">
  <img alt="Projecte" src="https://img.shields.io/badge/Projecte-Aitex_Datacenter-2E7D6B?style=for-the-badge">
  <img alt="CE" src="https://img.shields.io/badge/Criteris_Avaluaci%C3%B3-RA2.c_·_RA2.g-2E7D6B?style=for-the-badge">
</p>

Aquest document concreta la selecció raonada del **maquinari**, **programari** (sistemes operatius i aplicacions) i els **perfils professionals d'enginyeria** necessaris per dur a terme la **implantació inicial** de la infraestructura tecnològica d'**Aitex Datacenter**.

---

## 💻 1. Selecció de Recursos Materials: Maquinari (Hardware)

Tots els equips físics han estat seleccionats per complir els requisits d'alta disponibilitat, eliminació de punts únics de fallada (SPOF) i eficiència energètica en entorn de CPD:

| Categoria / Element | Quantitat | Funció a la infraestructura | Requisits principals | Justificació tècnica |
|---|:---:|---|---|---|
| **Encaminadors de Vora (Edge Routers)** | 2 unitats | Enrutament extern i connexió BGP redundant amb dos ISPs. | Xassís d'alta capacitat, doble font d'alimentació hot-swap, interfícies 10G/40G SFP+, taules de rutes BGP completes. | Garanteix la continuïtat del servei d'Internet; si un operador o router cau, el segon manté l'accés global. |
| **Tallafocs de Nova Generació (NGFW)** | 2 unitats | Protecció perimetral, prevenció d'intrusions (IPS) i DMZ. | Clúster Actiu/Passiu (HA), xifratge VPN IPsec/SSL, rendiment d'inspecció profunda (DPI) > 10 Gbps, mitigació anti-DDoS. | Filtra el trànsit maliciós abans que arribe als 320 servidors de producció o a la xarxa d'oficines. |
| **Commutadors de Nucli (Core Switches L3)** | 2 unitats | Enrutament inter-VLAN i distribució del trànsit d'alta velocitat. | Capa 3, amplada de banda de commutació > 1,2 Tbps, ports de fibra 10G/40G QSFP+, apilament virtual (VSS/Stacking). | Nucli d'alta velocitat sense colls d'ampolla per comunicar les 4 plantes de sales blanques amb la DMZ i l'exterior. |
| **Commutadors Top-of-Rack (Switches ToR L2+)** | 32 unitats | Commutació d'accés a cada armari rack de les sales blanques. | 48 ports Gigabit/10G RJ45 + 4 uplinks 10G SFP+, suport complet IEEE 802.1Q (VLANs), baix consum energètic. | 2 switches ToR per armari rack asseguren que cada servidor tinga doble connexió física a xarxa per redundància. |
| **Commutadors d'Oficina (Switches PoE L2+)** | 2 unitats | Connexió dels equips d'usuari, telefonia IP i punts d'accés Wi-Fi. | 48 ports Gigabit PoE+ (802.3at) fins a 370W, gestió SNMP, segmentació en VLANs. | Alimenta directament els 6 APs i els 16 telèfons VoIP a través del mateix cable de dades de xarxa. |
| **Punts d'Accés Wi-Fi (AP)** | 6 unitats | Cobertura sense fils corporativa a les 5 plantes de l'edifici. | Estàndard Wi-Fi 6 (802.11ax), suport multi-SSID, itinerància (*roaming*) ràpid 802.11r/k, xifratge WPA3-Enterprise. | Permet als tècnics desplaçar-se amb portàtils per les 4 sales blanques sense perdre connexió de diagnòstic. |
| **Servidors de Producció (Racks 1U)** | 320 unitats | Allotjament dels serveis de hosting B2B (*Half* i *Full-Server*). | Format Rack 1U, doble CPU 16-Core, 128 GB RAM ECC, discos NVMe en RAID-1, mòduls de gestió remota IPMI/iLO. | Densitat de procés òptima per a 80 servidors per planta, amb rendiment garantit per a bases de dades i pimes. |
| **Servidors de Gestió Interna** | 7 unitats | Serveis de domini (AD), monitoratge SIEM, ERP clients i backups. | Format Rack 2U, xassís d'alta capacitat d'emmagatzematge SAS (fins a 64 TB útils en RAID-6/ZFS), targetes 10G. | Redundància local de control i custòdia de les còpies de seguretat diàries i la telemetria BMS. |
| **Estacions de Treball Fixes (PCs)** | 13 unitats | Puestes de treball dels enginyers del NOC/SOC i administració. | Processador de 8 nuclis, 32 GB RAM, disc SSD 1 TB, targeta gràfica dedicada amb suport per a 2-4 monitors. | Indispensables per visualitzar mètriques en temps real, logs del SIEM i administrar múltiples terminals alhora. |
| **Ordinadors Portàtils Tècnics** | 5 unitats | Diagnòstic físic in situ i supervisió de sales blanques. | Xassís robust (*semi-rugged*), alta autonomia de bateria, ports Ethernet Gigabit nadius, resistents a pols i vibracions. | Mobilitat contínua dels 3 tècnics de manteniment i 2 de sistemes per atendre incidències a peu de rack. |
| **Videowall Sala de Control NOC** | 4 pantalles | Visualització centralitzada del trànsit, estat dels servidors i alarmes. | Pantalles industrials LED de 55" de marc ultraprim (0,88 mm) preparades per a funcionament ininterromput 24/7. | Permet als 4 operadors del SOC/NOC detectar anomalies de xarxa o caigudes de servei de forma instantània. |
| **Consoles KVM sobre IP** | 8 unitats | Accés fora de banda (OOBM) a nivell de maquinari i BIOS. | Consoles plegables 1U amb pantalla/teclat i passarel·la KVM-over-IP per accés remot xifrat. | Accés garantit als servidors encara que el sistema operatiu host haja sofert una caiguda o bloqueig crític. |
| **Sistemes de Resguard Elèctric (SAI/UPS)** | Mòduls N+1 | Protecció d'alimentació contra talls elèctrics i microtalls. | Tecnologia On-Line de doble conversió, autonomia mínima de 30 minuts fins a l'arrancada del generador dièsel. | Evita aturades imprevistes i avaries físiques als discs i fonts d'alimentació dels 327 servidors. |

---

## 🧩 2. Selecció de Recursos Materials: Programari (Sistemes Operatius i Aplicacions)

| Categoria | Programari Seleccionat | Quantitat / Llicències | Funció principal | Justificació tècnica |
|---|---|:---:|---|---|
| **Hipervisor de Virtualització** | **Proxmox VE Enterprise / VMware ESXi** | 320 llicències | Virtualització tipus 1 (*Bare-metal*) per als servidors de producció. | Permet crear màquines virtuals aïllades per al pla *Half-Server* amb migració en viu (*Live Migration*) i gestió via API. |
| **Sistema Operatiu de Servidor** | **Windows Server 2022 Datacenter** | 2 llicències | Controladors de Domini intern (Active Directory, DNS, GPO). | Estàndard indiscutible per a la gestió centralitzada d'usuaris, equips i polítiques de seguretat corporatives. |
| **Sistema Operatiu Open Source** | **Debian GNU/Linux 12 / Ubuntu Server** | 5 llicències / nodes | Servidors interns de DMZ (Web, DNS públic, Correu) i Monitoratge. | Màxima estabilitat, seguretat contrastada, baix consum de recursos i absència de costos recurrents de llicència. |
| **Sistemes Operatius Client** | **Windows 11 Pro i Ubuntu Desktop LTS** | 18 llicències | Sistema operatiu dels 13 PCs de sobretaula i 5 portàtils. | Compatibilitat amb eines ofimàtiques corporatives i consoles d'administració de xarxa per a enginyers. |
| **Gestió i Monitoratge SIEM** | **Zabbix Enterprise + Grafana / Wazuh** | 1 plataforma (Clúster) | Monitoratge de xarxa en temps real, logs de seguretat i alertes al Videowall. | Correlació d'esdeveniments de ciberseguretat, telemetria SNMP de switches i routers, i notificacions 24/7. |
| **Plataforma BMS (Edifici)** | **Schneider EcoStruxure / Eina Open BMS** | 1 plataforma | Gestió dels sensors IoT de temperatura, humitat, fums i consum de SAIs. | Monitoratge predictiu ambiental per prevenir sobreescalfaments a les 4 plantes de sales blanques. |
| **Portal Web, ERP i Facturació** | **Odoo Enterprise / Portal B2B Custom** | 1 instància en DMZ | Web comercial, contractació de plans, ticketing i facturació periòdica. | Automatitza la facturació mensual dels 1.152.000 € previstos i ofereix als clients un portal segur d'autoservei. |
| **Còpies de Seguretat** | **Veeam Backup & Replication** | 1 infraestructura | Còpies de seguretat immutables de servidors interns i snapshots de màquines virtuals. | Protecció contra atacs de *ransomware* mitjançant repositoris Linux amb bloqueig d'escriptura temporal. |
| **Seguretat Endpoint (EDR/Antivirus)**| **CrowdStrike Falcon / Bitdefender GravityZone** | 18 llicències | Protecció dels 13 PCs fixes i 5 portàtils contra amenaces avançades. | Detecció basada en comportament que evita que un lloc d'usuari compromès s'utilitze com a vector d'intrusió interna. |

---

## 👥 3. Selecció de Recursos Humans: Equip d'Implantació del Projecte

> [!IMPORTANT]
> **Distinció clau de rols:** Els següents professionals conformen **l'Equip Tècnic d'Implantació i Desplegament**. Es tracta d'enginyers i especialistes de projecte responsables d'executar el muntatge físic, el cablejat, el disseny de xarxa i la posada en marxa inicial.
> Aquests perfils **no s'han de confondre amb els 16 treballadors fixos d'Aitex Datacenter** (Direcció, Tècnics de Suport 24/7, Administradors de Sistemes i Manteniment), els quals assumiran l'operativa diària de l'empresa una vegada inaugurada la instal·lació.

| Perfil de l'Equip d'Implantació | Responsabilitats generals durant el projecte | Tasques clau assignades (Blocs 2 i 3) |
|---|---|---|
| 📐 **Cap de Projecte (Project Manager / Scrum Master)** | Liderar la planificació àgil (Scrum), coordinar l'equip, supervisar el pressupost i garantir el lliurament dels sprints. | Administració del tauler Trello, control de costos del pressupost, validació de requisits i comunicació amb el client/professorat. |
| 🌐 **Arquitecte de Xarxes (Network Engineer)** | Dissenyar la topologia lògica i física, definir el pla d'adreçament IP i configurar l'electrònica de xarxa. | Configuració dels Routers BGP, desplegament del clúster de tallafocs NGFW, creació de les 11 VLANs als switches Core i ToR. |
| 🐧 **Enginyer de Sistemes i Ciberseguretat (Systems & Security Engineer)** | Instal·lació dels hipervisors, desplegament de serveis a la DMZ, serveis de directori i polítiques de protecció. | Posada en marxa del Directori Actiu (AD), configuració de Proxmox/VMware, bastionat (*hardening*) de servidors i desplegament del SIEM. |
| 🔧 **Tècnic Especialista en CPD i Cablejat (Data Center & Cabling Tech)** | Muntatge físic del maquinari als racks, estesa de fibra òptica/coure, connexió elèctrica i certificació de cablatge. | Fixació dels 32 switches ToR, etiquetatge normalitzat segons TIA-606-B, connexió de consoles KVM i integració de sensors BMS. |
