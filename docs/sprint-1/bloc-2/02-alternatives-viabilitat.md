<h1 align="center">🔄 Tasca 2. Alternatives i Estudi de Viabilitat Tècnica i Econòmica</h1>

<p align="center">
  <img alt="Empresa" src="https://img.shields.io/badge/Aitex_Datacenter-1F3A5F?style=for-the-badge">
  <img alt="Bloc" src="https://img.shields.io/badge/Bloc_2-Disseny_i_Viabilitat-2E7D6B?style=for-the-badge">
  <img alt="Estat" src="https://img.shields.io/badge/Estat-Avaluaci%C3%B3_i_Dimensionament-D97706?style=for-the-badge">
  <img alt="CE" src="https://img.shields.io/badge/Criteris_Avaluaci%C3%B3-RA2.b_·_RA2.c_·_RA2.d_·_RA2.e-2E7D6B?style=for-the-badge">
</p>

<p align="center"><em>Anàlisi comparativa d'alternatives tecnològiques, justificació de la solució escollida, innovació aplicada i càlculs de dimensionament elèctric, tèrmic i financer per a Aitex Datacenter.</em></p>

---

## ⚖️ 1. Comparativa d'Alternatives Tecnològiques

Per resoldre el repte de connectivitat, distribució de càrrega i allotjament dels **320 servidors de producció** distribuïts en **4 plantes de sales blanques i 1 planta d'oficines**, s'han comparat dues alternatives integrals d'arquitectura:

| Concepte / Criteri | Alternativa A: Xarxa Tradicional de Coure (Cat6) i Servidors Independents | Alternativa B: Nucli de Fibra 10G/40G, Top-of-Rack (ToR), Redundància N+1 i Hipervisors (Escollida) |
|---|---|---|
| 🌐 **Topologia de Xarxa** | Cablejat de coure Cat6 directe des d'un repartidor central a cada armari de rack (1 sol camí físic). | Nucli de fibra òptica 10G/40G amb MLAG/Stacking i 2 switches *Top-of-Rack* (ToR) redundants per fila de racks. |
| 💻 **Gestió de Servidors** | Servidors físics autònoms sense capa de virtualització centralitzada ni doble enllaç LACP. | Clúster híbrid amb hipervisors (**Proxmox VE**) i doble targeta de xarxa en *bonding* LACP per a *Half/Full-Server*. |
| ⚡ **Infraestructura Crítica** | SAI bàsic sense redundància N+1 i climatització industrial convencional sense contenció de passadissos. | SAIs trifàsics modulars **N+1 (3×80 kW)**, grup electrògen 250 kVA, doble escomesa A+B i climatització de precisió CRAC **N+1**. |
| 👍 **Avantatges** | • Inversió inicial inferior (~15% menys en electrònica i redundància).<br>• Configuració de capa 2 més senzilla. | • **Alta Disponibilitat (Uptime 99,99%)** sense punts únics de fallada (SPOF).<br>• Eliminació de colls d'ampolla en trànsit Nord-Sud i Est-Oest.<br>• Escalabilitat instantània de *Half-Server* a *Full-Server*. |
| ⚠️ **Limitacions** | • Major latència i saturació en hores punta.<br>• Fallada d'un switch o unitat de clima atura tota una planta.<br>• Inviable per garantir SLAs B2B 24/7. | • CAPEX inicial més elevat per la duplicació d'equips crítics.<br>• Requereix enginyeria especialitzada per a la posada en marxa. |
| 💶 **Cost estimat (CAPEX base)** | ~1.040.000 € *(sense redundància N+1 ni fibra 10G/40G)* | **~1.220.000 €** *(projecte complet clau en mà: 320 servidors, xarxa, SAIs N+1, clima CRAC i mà d'obra; detall a la Tasca 6)* |

### 🎯 Decisió del grup i justificació
Es selecciona l'**Alternativa B**. En un CPD comercial amb 320 servidors B2B, l'estalvi inicial de l'Alternativa A no compensa el risc d'incompliment d'acords de nivell de servei (SLA) per talls elèctrics, sobreescalfament o caiguda d'enllaços. La fibra 10G/40G, el disseny ToR i la redundància elèctrica/tèrmica N+1 garanteixen continuïtat operativa real 24/7.

---

## 🚀 2. Aspecte Innovador Aplicable

Com a element diferenciador respecte a CPDs tradicionals de la comarca, **Aitex Datacenter** incorpora un **Sistema de Monitoratge Predictiu basat en Telemetria IoT i Correlació SIEM/BMS**:

* **Utilitat:** Correlaciona en temps real les dades de sondes tèrmiques als passadissos freds/calents, el consum elèctric per PDU intel·ligent i el trànsit de xarxa (Zabbix + Grafana + Wazuh) per detectar anomalies tèrmiques, degradació de fonts d'alimentació o atacs DDoS abans que provoquen una caiguda.
* **Valor afegit:** Publica un panell de transparència (*Status Page*) per als clients B2B i optimitza dinàmicament les consignes dels equips de climatització CRAC per mantindre el **PUE** al voltant d'**1,45**.

---

## ⚙️ 3. Càlculs de Viabilitat Tècnica (Potència, SAIs, Climatització i Redundància)

Per demostrar rigorosament que la infraestructura suporta els **320 servidors de producció**, els **7 servidors interns** i els **46 dispositius de xarxa**, s'han efectuat els següents càlculs de dimensionament:

### 3.1 Càlcul de Càrrega Elèctrica TI i Total de la Instal·lació

| Bloc d'Equipament | Unitats | Consum Mitjà / Unitat | Potència Mitjana (kW) | Potència Pic (kW) |
|---|:---:|:---:|:---:|:---:|
| Servidors de Producció 1U (`Dell PowerEdge R360`) | 320 | 280 W *(pic 400 W)* | 89,6 kW | 128,0 kW |
| Servidors de Gestió Interna 2U (`Dell PowerEdge R750xs`) | 7 | 400 W *(pic 600 W)* | 2,8 kW | 4,2 kW |
| Electrònica de Xarxa (2 Routers, 2 FW, 2 Core, 32 ToR, 2 PoE, 6 AP) | 46 | *Segons fitxa tècnica* | 4,0 kW | 6,0 kW |
| **Total Càrrega TI (Equipament Informàtic)** | **373** | — | **96,4 kW** | **138,2 kW** |

Aplicant un índex d'eficiència energètica **PUE (*Power Usage Effectiveness*) = 1,45** (gràcies a la contenció de passadissos freds/calents i SAIs d'alta eficiència) i un factor de potència $\cos\varphi = 0,90$:

* **Potència elèctrica total mitjana (TI + Climatització + Pèrdues):**
  $$P_{\text{total, mitjana}} = 96,4\text{ kW} \times 1,45 = \mathbf{139,78\text{ kW}} \quad \left(\frac{139,78}{0,90} \approx \mathbf{155,3\text{ kVA}}\right)$$
* **Potència elèctrica total en pic màxim (100 % càrrega simultània):**
  $$P_{\text{total, pic}} = 138,2\text{ kW} \times 1,45 = \mathbf{200,39\text{ kW}} \quad \left(\frac{200,39}{0,90} \approx \mathbf{222,7\text{ kVA}}\right)$$
* **Escomesa i Grup Electrògen:** Es dimensiona una escomesa/centre de transformació de **250 kVA** i un **Grup Electrògen Dièsel insonoritzat de 250 kVA** amb quadre de transferència automàtica (ATS) i dipòsit de combustible per a 48 hores de funcionament autònom, cobrint sobradament el pic màxim (222,7 kVA).

### 3.2 Dimensionament dels Sistemes d'Alimentació Ininterrompuda (SAIs / UPS)

Els SAIs protegeixen exclusivament la **Càrrega TI (96,4 kW mitjans / 138,2 kW pic)** i els equips crítics de control durant els talls de subministrament mentre arranca el grup electrògen (< 30 segons):

* **Solució seleccionada:** **3 × Schneider Electric Galaxy VS 80 kW (400V trifàsic)** en configuració redundant **N+1**.
  * **Capacitat útil garantida (N = 2 unitats):** $2 \times 80\text{ kW} = \mathbf{160\text{ kW}}$, superior tant a la càrrega mitjana (96,4 kW) com al pic màxim (138,2 kW).
  * **Tolerància a fallades (+1):** Si una unitat SAI entra en manteniment o falla, les altres dues absorbeixen el 100 % de la càrrega sense interrupció.
  * **Autonomia de bateries:** Banc de bateries extern dimensionat per a **15–20 minuts a càrrega nominal**, marge àmpliament suficient per a l'arrancada del grup electrògen i, en cas d'emergència extrema, un apagat ordenat dels hipervisors.
* **Distribució en Racks (Doble Branca A+B):** Tots els servidors i equips de xarxa compten amb **doble font d'alimentació *hot-swap*** connectades a **2 PDUs intel·ligents per armari** (Branca A i Branca B, 36 PDUs en total per als 18 racks).

### 3.3 Dimensionament de Climatització i Refrigeració (CRAC N+1)

Pràcticament el 100 % de la potència elèctrica consumida per la càrrega TI es dissipa en forma de calor ($1\text{ kW} = 3.412,14\text{ BTU/h}$):

* **Càrrega tèrmica total TI:**
  * Mitjana: $96,4\text{ kW} \times 3.412,14 \approx \mathbf{328.930\text{ BTU/h}}$
  * Pic màxim: $138,2\text{ kW} \times 3.412,14 \approx \mathbf{471.558\text{ BTU/h}}$
* **Càrrega tèrmica per cada Sala Blanca (4 plantes, 80 servidors + 8 switches ToR per planta):**
  * Mitjana per planta: $(80 \times 0,28) + (8 \times 0,075) = \mathbf{23,0\text{ kW}}$ ($\approx 78.480\text{ BTU/h}$)
  * Pic màxim per planta: $(80 \times 0,40) + (8 \times 0,10) = \mathbf{32,8\text{ kW}}$ ($\approx 111.918\text{ BTU/h}$)
* **Solució seleccionada:** **2 unitats de climatització de precisió CRAC de 35–40 kW frigorífics per planta** (`Schneider Electric Uniflair`, **8 unitats en total**), operant en esquema **N+1 per planta**.
  * En operació normal treballen al ~35–45 % de càrrega en alternança (màxima eficiència i menor desgast).
  * Si falla una unitat en qualsevol sala blanca, l'altra unitat de 35–40 kW cobreix el **100 % de la càrrega tèrmica pic (32,8 kW)** de la planta mantenint la temperatura entre 20 °C i 24 °C (estàndard ASHRAE TC 9.9).

### 3.4 Ocupació d'Armaris Rack (42U)
* **4 Sales Blanques:** 4 armaris 42U per planta (**16 armaris**). Cada armari allotja **20 servidors 1U + 2 switches ToR 1U + 2 panells de connexió + gestió de cables** ($\approx 26\text{U}$ ocupades, ~6 kW/rack, dins dels paràmetres òptims de refrigeració per aire).
* **Planta 1 (Sala de Control i Nucli):** **2 armaris 42U** per als 2 routers BGP, 2 firewalls, 2 switches Core L3, 2 switches PoE i els 7 servidors interns 2U. **Total: 18 armaris Rack 42U.**

---

## 📊 4. Resum de l'Estudi de Viabilitat en Quatre Dimensions

| Dimensió de Viabilitat | Anàlisi i Comprovació | Estat |
|---|---|:---:|
| 🏢 **Viabilitat d'Espais** | Edifici de 5 plantes a Alcoi: 1 planta d'oficines/control (2 racks) i 4 sales blanques (4 racks de 42U i 2 unitats CRAC per planta) amb espai per a passadissos freds/calents. | ✅ Viable |
| 👥 **Viabilitat de Coneixements** | Plantilla de 16 especialistes (Sistemes, Xarxes NOC/SOC, Manteniment físic i Administració) capacitats per operar l'arquitectura de xarxa, Proxmox i els sistemes BMS/CRAC. | ✅ Viable |
| ⚡ **Viabilitat Tècnica i Energètica** | Càlculs validats: càrrega TI (96,4–138,2 kW) coberta amb 3×80 kW SAIs N+1, grup electrògen 250 kVA, 8×CRAC 35–40 kW N+1 i 18 racks amb doble PDU A+B. | ✅ Viable |
| 💶 **Viabilitat Econòmica** | Amb un CAPEX complet d'**1.219.378 € (sense IVA)**, ingressos d'**1.152.000 €/any** (75 % ocupació) i un OPEX anual de **843.564 €/any**, s'obté un EBITDA de **308.436 €/any (26,8 %)** i un retorn de la inversió (**Payback**) realista d'uns **4 anys** (detall complet a [`06-pressupost.md`](06-pressupost.md)). | ✅ Viable |

---

## 📚 5. Fonts Tècniques Consultades

* **ASHRAE TC 9.9 (Thermal Guidelines for Data Processing Environments):** Rangs recomanats de temperatura (18–27 °C) i humitat per a sales blanques de CPDs.
* **Uptime Institute — Tier Standard: Topology:** Requisits de redundància N+1 en potència (SAIs/Generador) i refrigeració (CRAC).
* **Schneider Electric — Galaxy VS 80 kW Technical Specifications:** [Fitxa oficial GVSUPS80KHS](https://www.se.com/us/en/product/GVSUPS80KHS/galaxy-vs-ups-80kw-400v-for-external-batteries-startup-5x8/) i gamma [Uniflair Precision Cooling](https://www.se.com/ww/en/work/products/master-ranges/uniflair/).
* **Dell Technologies — PowerEdge R360 Environmental & Power Specs:** [Documentació oficial PowerEdge R360](https://www.dell.com/support/product-details/en-us/product/poweredge-r360/resources/manuals).
