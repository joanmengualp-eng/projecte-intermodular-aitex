<h1 align="center">🎯 Tasca 1. Identificació de les necessitats TIC</h1>

<p align="center">
  <img alt="Fase" src="https://img.shields.io/badge/Fase-An%C3%A0lisi-1F3A5F?style=for-the-badge">
  <img alt="Projecte" src="https://img.shields.io/badge/Projecte-Aitex_Datacenter-2E7D6B?style=for-the-badge">
  <img alt="CE" src="https://img.shields.io/badge/Criteris_Avaluaci%C3%B3-RA2.a_·_RA2.c-2E7D6B?style=for-the-badge">
</p>

A partir de l'anàlisi de l'estructura organitzativa, els 16 treballadors/es i les funcions establertes en el Bloc 1 d'**Aitex Datacenter**, s'han identificat i catalogat les necessitats tecnològiques fonamentals per garantir la continuïtat de negoci, la seguretat de les dades i l'operativa ininterrompuda 24/7 de la infraestructura del CPD.

---

## 📋 Taula de Necessitats TIC per Departament

La següent taula recull de manera exhaustiva les necessitats tècniques exigides, vinculant cada departament amb els usuaris afectats i la seua justificació específica:

| Departament | Àmbit / Necessitat TIC | Usuaris afectats | Justificació tècnica i operativa |
|---|---|---|---|
| 🌍 **Xarxes i Ciberseguretat** | **Connectivitat d'alta velocitat i accés a Internet redundant** | Tota la plantilla (16 usuaris) + 320 servidors de producció | Un CPD no pot dependre d'un sol operador. Es requereix connexió de fibra simètrica amb dos proveïdors d'Internet (ISPs independents) mitjançant protocol BGP per garantir disponibilitat ininterrompuda (99,99% Uptime) i balanceig de càrrega. |
| 🛡️ **Xarxes i Ciberseguretat** | **Seguretat perimetral i protecció anti-DDoS** | Equip NOC/SOC (4 usuaris) i trànsit de clients | Protecció contra atacs massius de denegació de servei (DDoS), escanejos de ports i intents d'intrusió. Es requereix un clúster de tallafocs de nova generació (NGFW) amb inspecció profunda de paquets (DPI) i sistemes de prevenció d'intrusions (IPS). |
| 🌐 **Xarxes i Ciberseguretat** | **Segmentació de xarxa i Zona Desmilitaritzada (DMZ)** | Clients externs i departaments interns | Aïllament físic i lògic mitjançant VLANs. Els servidors accessibles des de l'exterior s'han d'ubicar en una DMZ perquè, en cas de vulnerabilitat web, els atacants no puguen accedir a la xarxa corporativa interna ni a la xarxa de gestió. |
| 🔑 **Sistemes i Suport Tècnic** | **Comptes d'usuari i gestió centralitzada d'identitats** | Tota la plantilla (16 treballadors/es) | Implementació d'un servei de directori (Directori Actiu / LDAP) per gestionar usuaris, contrasenyes, directrius de seguretat (GPO), rols i permisos d'accés als equips i servidors des d'un únic punt administratiu. |
| 📂 **Administració i Sistemes** | **Compartició de fitxers i emmagatzematge segur** | Administració (3) i Tècnics de Sistemes (6) | Necessitat d'un servidor de fitxers intern (NAS/SAN) amb sistemes RAID i permisos ACL per emmagatzemar contractes de clients B2B, facturació, documentació tècnica, procediments ISO i imatges ISO de sistemes operatius. |
| 🖨️ **Administració i Manteniment** | **Impressió en xarxa i etiquetatge tècnic d'infraestructura** | Administració (3) i Tècnics de camp (5) | • **Impressió ofimàtica:** Impressora multifunció làser en xarxa segura amb codi d'autenticació per a la impressió de contractes legals i albarans.<br>• **Impressió tècnica:** Impressores tèrmiques d'etiquetes resistents a altes temperatures per identificar racks, servidors i el cablatge estructurat segons la norma ANSI/TIA-606-B. |
| 💻 **Comercial i Sistemes** | **Web corporativa i portal d'autoservei per a clients** | Clients B2B, Comercial (1) i Suport (2) | Portal web allotjat a la DMZ amb passarel·la xifrada (HTTPS/TLS) on els clients externs consulten l'estat dels seus servidors, sol·liciten ampliacions de pla (*Half-Server* a *Full-Server*) i obrin tiquets de suport. |
| 🔧 **Sistemes i Suport Tècnic** | **Accés remot segur i xarxa fora de banda (OOBM)** | Administradors de Sistemes i Tècnics N1/N2 (6 usuaris) | Consoles KVM sobre IP i xarxes dedicades per gestionar directament la BIOS i els controladors IPMI/iLO/iDRAC dels 320 servidors físics, permetent la recuperació fins i tot si cau el sistema operatiu o la xarxa principal. |
| ⚡ **Manteniment i Infraestructura** | **Monitoratge ambiental i telemetria de sensors (IoT)** | Manteniment (3 usuaris) i Operadors NOC (4 usuaris) | Desplegament de sondes de temperatura, humitat, detecció d'inundació i consum elèctric dels SAIs connectades a un sistema BMS/SIEM per alertar automàticament de desviacions tèrmiques abans que es produïsquen danys al maquinari. |
| 📶 **Transversal (Tots)** | **Xarxa sense fils (Wi-Fi) corporativa i de visites** | 16 empleats i visites/auditors externs | Punts d'accés Wi-Fi 6 de classe empresarial amb dos SSIDs aïllats: un amb autenticació WPA3-Enterprise (802.1X/RADIUS) per als portàtils tècnics en mobilitat per les 5 plantes, i un altre amb portal captiu i aïllament d'usuaris per a visites sense accés a la xarxa local. |

---

## 📚 Fonts consultades per a la identificació de necessitats

Per fonamentar tècnicament cadascuna de les necessitats anteriors s'han consultat les següents referències i estàndards oficials de la indústria:

1. **Uptime Institute - Data Center Site Infrastructure Tier Standard:** Recomanacions de redundància, xarxes concurrentment mantenibles i telemetria ambiental continuada per a CPDs Tier II i Tier III.
2. **ISO/IEC 27001 i Esquema Nacional de Seguretat (ENS):** Requisits per al control d'accés d'usuaris, autenticació robusta, registre d'activitat (logs), segmentació de xarxes perimetrals (DMZ) i protecció contra malware.
3. **Cisco Data Center Design Guides (CVD):** Metodologia per al desplegament de xarxes de gestió fora de banda (*Out-of-Band Management* - OOBM) i xarxes jeràrquiques de commutació d'alta densitat.
4. **Norma ANSI/TIA-606-B:** Estàndard d'administració i etiquetatge per a infraestructures de telecomunicacions en centres de dades.
5. **Reglament General de Protecció de Dades (RGPD / LOPDGDD):** Directrius sobre aïllament lògic i xifratge en repòs i trànsit per a dades de tercers (clients B2B).
