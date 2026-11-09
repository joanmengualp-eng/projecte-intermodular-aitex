<h1 align="center">🔄 Alternatives i Estudi de Viabilitat</h1>

<p align="center">
  <img alt="Empresa" src="https://img.shields.io/badge/Aitex_Datacenter-1F3A5F?style=for-the-badge">
  <img alt="Bloc" src="https://img.shields.io/badge/Bloc_2-Disseny_i_Viabilitat-2E7D6B?style=for-the-badge">
  <img alt="Fase" src="https://img.shields.io/badge/Estat-Avaluaci%C3%B3_d'Alternatives-D97706?style=for-the-badge">
  <img alt="CE" src="https://img.shields.io/badge/Criteris_Avaluaci%C3%B3-RA2.b_·_RA2.c_·_RA2.d_·_RA2.e-2E7D6B?style=for-the-badge">
</p>

<p align="center"><em>Anàlisi comparativa d'alternatives tecnològiques, selecció de la solució òptima, innovació aplicada i estudi de viabilitat tècnica i econòmica per a Aitex Datacenter.</em></p>

---

## ⚖️ 1. Comparativa d'Alternatives Tecnològiques

Per resoldre el repte de connectivitat, distribució de càrrega i allotjament de l'infraestructura de les **4 plantes de sales blanques i la planta d'oficines**, s'han estudiat dues alternatives tècniques principals:

| Concepte / Criteri | Alternativa A: Xarxa Tradicional de Coure (Cat6) i Servidors Independents | Alternativa B: Xarxa de Nucli de Fibra 10G, Top-of-Rack (ToR) i Hipervisors (Escollida) |
|---|---|---|
| 🌐 **Topologia de Xarxa** | Cablejat de coure Cat6 directe des d'un repartidor central a cada armari de rack. | Nucli de fibra òptica d'alta velocitat (10G) i switches *Top-of-Rack* (ToR) redundants per fila de racks. |
| 💻 **Gestió de Servidors** | Servidors físics completament autònoms sense capa de virtualització centralitzada prèvia. | Infraestructura híbrida amb hipervisors (Proxmox/VMware) per a la flexibilitat *Half/Full-Server*. |
| 👍 **Avantatges** | • Cost inicial de compra de materials lleugerament inferior.<br>• Configuració inicial més simple a nivell de commutació. | • **Alta Disponibilitat (Uptime 99,99%)**.<br>• Eliminació de colls d'ampolla en el trànsit de dades.<br>• Escalabilitat instantània per als clients B2B. |
| ⚠️ **Limitacions** | • Major latència en hores de punta.<br>• Si cau el switch principal, s'afecta tota la planta.<br>• Dificultat per migrar o escalar recursos de manera ràpida. | • Inversió inicial en maquinari i electrònica de xarxa més elevada.<br>• Requereix personal altament qualificat per a la seua configuració. |
| 💶 **Cost aproximat** | ~150.000 € (Infrastructura de xarxa i servidors base) | ~210.000 € (Inclou redundància, fibra i equips d'alta gamma) |
| ⚙️ **Requisits tècnics** | Canals de cablejat estàndard i switches de capa 2 no gestionables o bàsics. | Canals de fibra òptica, switches gestionables capa 3, consoles KVM i redundància elèctrica. |

### 🎯 Decisió del grup i justificació
Es selecciona l'**Alternativa B**. Tot i suposar un cost inicial superior, un Centre de Processament de Dades (CPD) B2B orientat a l'alta disponibilitat 24/7 no pot permetre's colls d'ampolla ni caigudes per fallades de xarxa. La combinació de fibra 10G amb *Top-of-Rack* garanteix la robustesa necessària per als 320 servidors de producció.

---

## 🚀 2. Aspecte Innovador Aplicable

Com a element diferenciador respecte a CPDs tradicionals de la zona, **Aitex Datacenter** incorporarà un **Sistema de Monitoratge Predictiu basat en Anàlisi de Trànsit i Sensors IoT (Intel·ligència de Sistemes)**:

* **Utilitat:** Permet anticipar-se a fallades de maquinari o atacs DDoS mitjançant la correlació en temps real de mètriques de temperatura de les sales blanques, consum elèctric dels racks i patrons de trànsit de xarxa al NOC/SOC.
* **Valor afegit:** Ofereix als clients B2B un panell de transparència d'estat (*Status Page*) personalitzat, augmentant la confiança en el servei d'allotjament crític.

---

## 📊 3. Estudi de Viabilitat

L'estudi de viabilitat demostra que el projecte és plenament viable i coherent en quatre dimensions clau:

| Dimensió de Viabilitat | Anàlisi i Comprovació | Estat |
|---|---|---|
| 🏢 **Viabilitat d'Espais** | L'edifici de 5 plantes d'Alcoi està distribuït de manera òptima: 1 planta d'oficines/control i 4 plantes de sales blanques dedicades exclusivamente a albergar 80 servidors en rack cadascuna. | ✅ Viable |
| 👥 **Viabilitat de Coneixements** | La plantilla de 16 treballadors/es (especialistes en sistemes, xarxes, ciberseguretat NOC/SOC i manteniment) posseeix les competències tècniques necessàries per operar l'arquitectura escollida. | ✅ Viable |
| 💻 **Viabilitat d'Equips** | S'han dimensionat correctament 18 equips d'usuari, 327 servidors totals (320 productius + 7 interns) i 46 dispositius de xarxa, assegurant compatibilitat tecnològica total. | ✅ Viable |
| 💶 **Viabilitat Pressupostària** | El volum de negoci anual estimat de **1.152.000 €** (basat en el 75% d'ocupació de servidors a 400 €/mes) garanteix sobradament la amortització de la inversió tecnològica i els costos operatius. | ✅ Viable |
