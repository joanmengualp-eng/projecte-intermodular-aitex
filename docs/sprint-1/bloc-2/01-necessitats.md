<h1 align="center">🎯 Tasca 1. Identificació de les necessitats TIC</h1>

<p align="center">
  <img alt="Fase" src="https://img.shields.io/badge/Fase-An%C3%A0lisi-1F3A5F?style=for-the-badge">
  <img alt="Projecte" src="https://img.shields.io/badge/Projecte-Aitex_Datacenter-2E7D6B?style=for-the-badge">
</p>

Per garantir l'operativa ininterrompuda d'**Aitex Datacenter**, s'han identificat les següents necessitats TIC basades en els rols dels 16 treballadors i en els requisits d'un Centre de Processament de Dades d'Alta Disponibilitat (Uptime).

---

## 📋 Taula de Necessitats per Departament

| Departament | Necessitat TIC | Usuaris Afectats | Justificació (Per què es necessita?) |
|---|---|---|---|
| 🔒 **Xarxes i Ciberseguretat** | **1. Segmentació i Seguretat Perimetral (DMZ)** | Equip SOC/NOC (4 usuaris) + Tots els clients | Cal aïllar els servidors web corporatius i el portal de clients (DMZ) de la xarxa interna de gestió del Datacenter per evitar accessos no autoritzats. |
| 🛠️ **Sistemes i Suport Tècnic** | **2. Accés Remot Segur i Fora de Banda (OOBM)** | Sysadmins i Tècnics N1/N2 (6 usuaris) | Necessitat de gestionar els 320 servidors físics mitjançant consoles KVM sobre IP i xarxes VPN de gestió, fins i tot si la xarxa principal del client cau. |
| 🛠️ **Sistemes i Suport Tècnic** | **3. Gestió centralitzada d'usuaris i permisos** | Tota la plantilla interna (16 usuaris) | S'ha d'implementar un Directori Actiu (AD/LDAP) per centralitzar les credencials d'accés als PCs, carpetes compartides i correu corporatiu. |
| ⚡ **Manteniment i Infraestructura** | **4. Monitoratge de Xarxa i Sensors (IoT)** | Manteniment (3 usuaris) i NOC (4 usuaris) | Obligatori per rebre alertes al *Videowall* i dispositius mòbils sobre pujades de temperatura a les sales blanques o talls elèctrics (Programari BMS/SIEM). |
| 👔 **Administració i Comercial** | **5. Compartició de fitxers i Portal Web** | Administració (3 usuaris) i Clients B2B | Necessiten accés a Internet d'alta velocitat, un servidor de fitxers intern per a contractes/factures i una web corporativa perquè els clients sol·liciten escalabilitat. |
| **Transversal** (Tots) | **6. Xarxa Wi-Fi Corporativa i de Visites** | 16 empleats + Clients/Visites | Wi-Fi segura amb xarxes separades (VLANs): una per als portàtils dels tècnics en mobilitat per les plantes i una altra aïllada per a visitants. |

---

## 📚 Fonts consultades

Per a la identificació d'aquestes necessitats s'han pres com a referència estàndards de la indústria dels CPD:
1. **Uptime Institute (Tier Standard):** Requisits per a la topologia de xarxa i monitoratge ambiental continu.
2. **Bones pràctiques ISO/IEC 27001:** Polítiques de control d'accés lògic (Directori Actiu) i segmentació de xarxes (DMZ).
3. **Documentació de disseny de xarxa de Cisco (Cisco Validated Designs per a Data Centers):** Implementació de xarxes de gestió *Out-of-Band* (OOB).
