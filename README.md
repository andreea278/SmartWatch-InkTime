# SmartWatch InkTime 

Proiect de dezvoltare a unui ceas inteligent bazat pe microcontrolerul Nordic nRF52840, echipat cu un afișaj E-Ink și senzori pentru monitorizarea activității.

---

## 1. Diagramă Bloc

![Block Diagram](./Images/block_diagram.png)

---

## 2. Bill of Materials (BOM)

| Componentă | Model / Valoare | Capsulă | Datasheet | Link JLC Parts |
| :--- | :--- | :--- | :--- | :--- |
| **MCU (U1)** | nRF52840_QF | AQFN-74 | [Datasheet](https://infocenter.nordicsemi.com/pdf/nRF52840_PS_v1.0.pdf) | [C190733](https://jlcpcb.com/parts/componentSearch?searchMetas=nRF52840) |
| **IMU (IC3)** | BMA421 | LGA-12 | [Datasheet](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bma423-ds000.pdf) | [C290940](https://jlcpcb.com/parts/componentSearch?searchMetas=BMA423) |
| **Charger (IC1)**| BQ25180YBGR | BGA-8 | [Datasheet](https://www.ti.com/lit/ds/symlink/bq25180.pdf) | [C5176378](https://jlcpcb.com/parts/componentSearch?searchMetas=BQ25180) |
| **Haptic (IC2)** | DRV2605YZFR | BGA-9 | [Datasheet](https://www.ti.com/lit/ds/symlink/drv2605.pdf) | [C63740](https://jlcpcb.com/parts/componentSearch?searchMetas=DRV2605) |
| **Buck-Boost (IC9)**| RT6160AWSC | BGA-15 | [Datasheet](https://www.richtek.com/assets/product_file/RT6160A/DS6160A-02.pdf) | [C2834372](https://jlcpcb.com/parts/componentSearch?searchMetas=RT6160) |
| **Fuel Gauge (U3)**| MAX17048G+T10 | SON-8 | [Datasheet](https://datasheets.maximintegrated.com/en/ds/MAX17048-MAX17049.pdf) | [C48255](https://jlcpcb.com/parts/componentSearch?searchMetas=MAX17048) |
| **Display Conn.** | 503480-2400 | FPC-24 | [Datasheet](https://www.molex.com/pdm_docs/sd/5034802400_sd.pdf) | [C73696](https://jlcpcb.com/parts/componentSearch?searchMetas=503480-2400) |
| **Antenna** | 2450AT18B100E| SMD | [Datasheet](https://www.johansontechnology.com/datasheets/2450AT18B100.pdf) | [C95123](https://jlcpcb.com/parts/componentSearch?searchMetas=2450AT18B100) |

---

## 3. Descrierea Funcționalității Hardware

### Componente Principale:
* **nRF52840:** Microcontroler cu suport Bluetooth 5.0, ales pentru consumul redus (Ultra-low power) și perifericele ADC/SPI complexe.
* **Display E-Ink:** Ecran cu consum zero în stare statică, oferind lizibilitate excelentă în lumină naturală.
* **Management Energie:** Încărcare prin USB-C și monitorizarea tensiunii bateriei printr-un divizor rezistiv conectat la un pin ADC.

### Comunicație și Procesare:
* **Interfață SPI:** Utilizată pentru refresh-ul ecranului (viteză ~4-8 MHz).
* **Interfață I2C:** Utilizată pentru citirea datelor de mișcare (accelerometru/giroscop).
* **Consum estimat:**
  * System OFF (Deep Sleep): ~3 µA
  * Activitate Bluetooth: ~15 µA (medie)
  * Refresh ecran: ~2-3 mA (vârf pe durată scurtă)

---

## 4. Mapare Pini nRF52840

| Pin MCU | Funcție | Componentă | Motiv / Detalii |
| :--- | :--- | :--- | :--- |
| **P0.xx** | SPI_MOSI | Display E-Ink | Linie de date principală pentru afișaj |
| **P0.yy** | SPI_SCK | Display E-Ink | Ceasul magistralei SPI |
| **P0.zz** | I2C_SDA | Senzor IMU | Magistrală de date senzori (cu Pull-up) |
| **P0.aa** | I2C_SCL | Senzor IMU | Ceas magistrală I2C |
| **P0.04** | AIN2 (ADC) | Baterie | Citire analogică a tensiunii acumulatorului |

---
## 5. Detalii Design PCB (Layout)

### Specificații Placă
* **Straturi:** 2 straturi (Top/Bottom).
* **Dimensiuni trasee:** 0.15mm semnale / 0.3mm alimentare.
* **Via sizes:** 0.2mm (Drill) / 0.35mm (Diameter).

### Decizii de Design și Erori DRC Acceptate
* **Copper Clearance (Aprobat):** În zona fan-out-ului pentru nRF52840, clearance-ul a fost redus manual sub 0.15mm pentru a permite rutarea corectă a pinilor AQFN cu pitch mic.
* **Antenna Keep-out:** S-a realizat o decupare a planului de masă sub antena ceramică pentru a maximiza eficiența RF, conform recomandărilor de design Nordic.

---


### PCB
| Top Layer | Bottom Layer |
|:---:|:---:|
| ![Top Layer](./Images/TopLayer.png) | ![Bottom Layer](./Images/BottomLayer.png) |
| **Inner Layer GND** | **Inner Layer Signal** |
| ![InnerLayer](./Images/InnerLayer.png) | ![InnerLayer](./Images/InnerLayer63Signal.png) |


---

## 6. Structură Repository
* `Hardware/` - Fișierele sursă `.sch` și `.brd` (Autodesk Fusion/Eagle).
* `Manufacturing/` - Fișiere Gerber, BOM și Pick & Place.
* `Mechanical/` - Modelul 3D al dispozitivului în format `.f3d` și `.step`.
* `Images/` - Randări și screenshot-uri ale proiectului.
