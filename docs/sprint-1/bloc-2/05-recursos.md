<h1 align="center">📦 Tasca 5. Selecció de recursos materials i humans</h1>

<p align="center">
  <img alt="Fase" src="https://img.shields.io/badge/Fase-Planificaci%C3%B3-1F3A5F?style=for-the-badge">
  <img alt="Projecte" src="https://img.shields.io/badge/Projecte-Aitex_Datacenter-2E7D6B?style=for-the-badge">
</p>

Aquest document concreta el maquinari, programari i els perfils professionals necessaris per dur a terme la **implantació inicial** de la infraestructura d'Aitex Datacenter.

---

## 💻 1. Selecció de Recursos Materials (Maquinari i Programari)

| Element / Recurs | Quantitat | Funció a la infraestructura | Requisits principals i Justificació |
|---|:---:|---|---|
| **Routers de Vora** | 2 | Connexió amb ISPs i enrutament extern. | Suport per a protocol BGP, ports de fibra 10/40 Gbps, alta disponibilitat. |
| **Firewalls (Maquinari)** | 2 | Protecció perimetral i segmentació de la DMZ. | Capacitat d'inspecció profunda de paquets (DPI), prevenció d'intrusions (IPS) i anti-DDoS. |
| **Switches Core (L3)** | 2 | Nucli de la xarxa, encaminament inter-VLAN. | Commutadors capa 3, ports de fibra SFP+, apilament (Stacking/VSS) per redundància. |
| **Switches Accés (ToR)** | 32 | Connexió directa als 320 servidors dels clients. | 48 ports Gigabit/10G per unitat, suport IEEE 802.1Q (VLANs), baix consum energètic. |
| **Switches Oficina (PoE)** | 2 | Connexió d'equips d'usuari, telèfons i APs Wi-Fi. | Suport PoE (Power over Ethernet) per alimentar antenes i telèfons IP sense cables de corrent extra. |
| **Punts d'Accés (AP)** | 6 | Xarxa Wi-Fi corporativa i per a tècnics en mobilitat. | Wi-Fi 6, capacitat de múltiples SSIDs (Gestió, Visites) i itinerància (*roaming*) fluida entre plantes. |
| **Servidors de Producció** | 320 | Allotjament dels plans *Full-Server* i *Half-Server*. | Format *Rack* 1U/2U, doble font d'alimentació, discos NVMe en RAID, mòduls IPMI/iLO per gestió remota. |
| **Servidors de Gestió** | 7 | Directori Actiu, Web, Monitoratge i Còpies. | Alta capacitat d'emmagatzematge intern (SAS/SATA) per a *backups* i redundància lògica. |
| **Equips Clients (PCs)** | 13 | Estacions de treball per als tècnics SOC/NOC i administració. | Processadors i gràfiques d'alt rendiment per suportar el *Videowall* i la virtualització d'escriptoris. |
| **Portàtils Robustos** | 5 | Equips mòbils per als tècnics d'infraestructura. | Resistents a colps, pols i variacions tèrmiques (dins de les sales blanques). |
| **SO de Virtualització** | 320 llicències | Crear les instàncies de *Half-Server* (Proxmox/VMware). | Hipervisor tipus 1 (*Bare-metal*) per optimitzar el rendiment del maquinari i garantir l'aïllament entre clients. |
| **SO de Servidors Interns** | 7 llicències | Executar l'AD (Windows Server) i eines Linux. | Estabilitat, compatibilitat amb eines de monitoratge de xarxa i gestió centralitzada de polítiques de domini. |

---

## 👥 2. Selecció de Recursos Humans (Equip d'Implantació)

> [!WARNING]
> Aquests perfils corresponen a **l'equip de desplegament i projecte** (que executarà la instal·lació inicial i la configuració de la xarxa i el maquinari), no s'han de confondre amb els 16 treballadors fixos que operaran l'empresa una vegada inaugurada.

Per portar a terme aquest projecte intermodular, es requereixen els següents rols durant la fase d'implantació:

| Perfil de l'Equip d'Implantació | Responsabilitats en aquest projecte (Blocs 2 i 3) |
|---|---|
| 📐 **Cap de Projecte (Project Manager)** | Planificar les tasques a Trello, controlar el pressupost econòmic, assegurar que es compleixen els terminis (sprints) i documentar l'estudi de viabilitat. |
| 🌐 **Arquitecte de Xarxes (Network Engineer)** | Dissenyar el mapa lògic i físic de la xarxa, definir el pla d'adreçament IP (Tasca 4), crear les subxarxes (VLANs) i configurar els Routers, Firewalls i Switches. |
| 🐧 **Enginyer de Sistemes (Systems Engineer)** | Instal·lar els hipervisors (Proxmox/VMware) als servidors inicials, configurar el Directori Actiu, desplegar la DMZ i verificar la connectivitat del portal web. |
| 🔧 **Tècnic/a de CPD (Hardware & Cabling)** | Responsable del muntatge físic als armaris *rack*, la implementació del cablejat estructurat, l'etiquetatge dels cables i la instal·lació dels Sistemes d'Alimentació Ininterrompuda (SAI). |

L'equip treballarà de manera coordinada seguint metodologies àgils (Scrum) per garantir que la infraestructura quede llesta per a la segona meitat del projecte (instal·lació i configuració tècnica al laboratori).
