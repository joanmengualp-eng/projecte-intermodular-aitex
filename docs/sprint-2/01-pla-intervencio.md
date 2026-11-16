<h1 align="center">📋 Tasca 1. Pla d'intervenció i organització del treball</h1>

<p align="center">
  <img alt="Sprint" src="https://img.shields.io/badge/Sprint_2-Implantaci%C3%B3_de_la_infraestructura-1F3A5F?style=for-the-badge">
  <img alt="Projecte" src="https://img.shields.io/badge/Projecte-Aitex_Datacenter-2E7D6B?style=for-the-badge">
  <img alt="CE" src="https://img.shields.io/badge/Criteris_Avaluaci%C3%B3-RA3.a--i_·_RA4.a-D97706?style=for-the-badge">
</p>

---

## 🎯 1. Abast

Es munta en VirtualBox una versió reduïda de la infraestructura del [Sprint 1](../sprint-1/bloc-2/03-disseny-xarxa.md) amb **5 màquines virtuals**. La plantilla es representa amb dos clients de prova.

| Màquina | Sistema | Xarxa | IP prevista |
|---|---|---|---|
| `srv-comunicacions` | Ubuntu Server 24.04 LTS | NAT + LAN + DMZ | `10.0.2.15` (NAT) · `192.168.40.1` · `192.168.20.1` |
| `srv-domini-1` | Windows Server 2022 | LAN | `192.168.40.5` (fixa) |
| `pc-admin` | Windows 11 Pro | LAN | DHCP (`192.168.40.50 – .200`) |
| `pc-sistemes-1` | Ubuntu Desktop 24.04 LTS | LAN | DHCP (`192.168.40.50 – .200`) |
| `srv-web` | Ubuntu Server 24.04 LTS | DMZ | `192.168.20.10` (fixa) |

- **LAN** (`aitex-lan`): `192.168.40.0/24`, la VLAN 40 de la SA1. **DMZ** (`aitex-dmz`): `192.168.20.0/24`, la VLAN 20 de la SA1.
- **Canvi respecte a la SA1:** el laboratori només té una LAN, així que `srv-domini-1` passa de `192.168.30.10` a `192.168.40.5`.
- **Sprint 3:** en `srv-domini-1` es crearà el domini `intra.aitexdatacenter.com` amb una unitat organitzativa per departament. En aquest sprint només es deixa el servidor preparat (nom, IP fixa i instantània).

---

## 📅 2. Tasques, dependències i temps

**5 setmanes · 10 hores** (una sessió de 2 h per setmana).

| Setm. | Tasca | Depèn de | Temps | Responsable | Col·laborador |
|:-:|---|---|:-:|---|---|
| 1 | **T1.** Pla d'intervenció, Trello i preparació de l'entorn (ISO i VirtualBox) | — | 2 h | Àlex Calabuig | [PENDENT] |
| 2 – 3 | **T2.** Creació de les 5 màquines i instal·lació dels sistemes | T1 | 3 h | [PENDENT] | [PENDENT] |
| 3 | **T3.** LAN, DMZ, noms, IP fixes i diagrama actualitzat | T2 | 1 h | [PENDENT] | [PENDENT] |
| 4 | **T4.** Encaminament i reenviament IPv4 persistent | T3 | 1 h | [PENDENT] | [PENDENT] |
| 4 | **T5.** Servei DHCP a la LAN | T3, T4 | 1 h | [PENDENT] | [PENDENT] |
| 5 | **T6.** Proves de connectivitat | T4, T5 | 1,5 h | [PENDENT] | [PENDENT] |
| 1 – 5 | **T7.** Seguiment, incidències i revisió final | Totes | 0,5 h | Tots dos | — |

**Camí crític:** T2 → T3 → T4 → T5 → T6. Si la T2 es retarda, es retarda tota la resta.

---

## 🧰 3. Recursos

### Programari
VirtualBox 7.x, ISO d'Ubuntu Server 24.04 LTS, Ubuntu Desktop 24.04 LTS, Windows Server 2022 (avaluació) i Windows 11 Pro. Les ISO es descarreguen de les webs oficials. També s'utilitzen GitHub, Trello i un editor de diagrames.

### Màquines virtuals

| Màquina | vCPU | RAM | Disc |
|---|:-:|:-:|:-:|
| `srv-comunicacions` | 1 | 1 GB | 15 GB |
| `srv-domini-1` | 2 | 4 GB | 50 GB |
| `pc-admin` | 2 | 4 GB | 64 GB |
| `pc-sistemes-1` | 2 | 3 GB | 25 GB |
| `srv-web` | 1 | 1 GB | 15 GB |
| **Total** | **8** | **13 GB** | **169 GB** (discos dinàmics) |

### Ordinadors amfitrions

| Ordinador | RAM | Disc lliure | Ús |
|---|:-:|:-:|---|
| [PENDENT: ordinador d'Àlex] | [PENDENT] | [PENDENT] | [PENDENT] |
| [PENDENT: ordinador de Joan] | [PENDENT] | [PENDENT] | [PENDENT] |

L'amfitrió que execute les màquines necessita almenys **16 GB de RAM**, uns **170 GB lliures** i la virtualització activada a la BIOS.

### Distribució de les màquines
Les xarxes internes de VirtualBox amb el mateix nom en dos ordinadors **no es connecten entre si**. Per això les 5 màquines funcionaran juntes en **un sol amfitrió**. Si algú instal·la una màquina al seu ordinador, l'exporta en `.ova` i s'importa a l'amfitrió principal generant noves adreces MAC. No s'utilitzen adaptadors en pont, perquè cap equip pot eixir a l'exterior sense passar per `srv-comunicacions`.

---

## 🔐 4. Permisos i autoritzacions

- **Professorat:** versions dels sistemes i ús de llicències d'avaluació de Windows.
- **Propietari del repositori:** accés del professorat al repositori privat i al tauler de Trello.
- **Norma de l'enunciat:** no es pugen ISO, discos de màquines, contrasenyes ni dades sensibles a GitHub.

---

## ⚠️ 5. Riscos i mesures preventives

| Risc | Mesura preventiva | Protecció o recuperació | Norma de l'aula |
|---|---|---|---|
| Treball prolongat amb pantalles | Pausa de 5 min cada hora; pantalla a l'altura dels ulls | Cadira regulable | Ergonomia en l'ús dels equips |
| Connexions elèctriques | No sobrecarregar regletes; cables fora del pas; no manipular equips encesos | Interruptor general; avisar el professorat | No manipular la instal·lació elèctrica |
| Pèrdua de dades | Instantània abans de cada canvi important; commits freqüents | Instantànies, còpia `.ova` en disc extern, GitHub | Desar i fer còpia del treball |
| Canvis de xarxa que afecten altres equips | Només NAT i xarxes internes; DHCP només a la LAN | Desactivar l'adaptador i tornar a la instantània | No modificar la xarxa de l'aula |

---

## 🧯 6. Imprevistos i solucions

| Imprevist | Solució prevista |
|---|---|
| **Falta de RAM** | Reduir la RAM dels clients i arrancar només les màquines necessàries per a cada prova |
| **Espai insuficient** | Discos dinàmics, esborrar instantànies antigues o moure les màquines a un disc extern |
| **Una instal·lació no arranca** | Activar VT-x/AMD-V a la BIOS, tornar a descarregar l'ISO i, en Windows 11, activar EFI i TPM 2.0 |

---

## 🗂️ 7. Trello i seguiment

**Tauler:** https://trello.com/b/GXiasy4l/projecte-intermodular-aitex
- **Llistes:** Pendent · En procés · Finalitzat.
- **Targetes:** una per tasca de la secció 2, amb el seu responsable i una descripció breu.
- **Al final de cada sessió:** es mouen les targetes, s'anota al [registre de seguiment](07-seguiment-incidencies.md) què s'ha fet, el temps i els problemes, i es compara amb la secció 2.
- **Desviacions:** si un retard afecta el lliurament, s'informa el professorat a la sessió següent i es registra l'ajust acordat.
