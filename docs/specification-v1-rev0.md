# CraftVia MakerCard-S v1 Rev0 Specification

> [!WARNING]
> **Status: Frozen design, unverified hardware**  
> Rev0 has been ordered but has not been assembled, powered, or electrically validated. Prototype-field connectivity, mechanical fit, and the integrated Core circuit all require first-article verification.

## 1. Identity and authority

| Item | Value |
| --- | --- |
| Product | CraftVia MakerCard-S |
| Product version | v1 |
| PCB revision | Rev0 |
| MCU | STM32G031K8T6 |
| CAD | EasyEDA |
| Frozen editable source | [`../source/CraftVia-MakerCard-S-v1-Rev0.eprj2`](../source/CraftVia-MakerCard-S-v1-Rev0.eprj2) |
| Fabrication master | [`../fabrication/Gerber_PCB1_2026-09-30.zip`](../fabrication/Gerber_PCB1_2026-09-30.zip) |
| Release record | [`../RELEASE_NOTES.md`](../RELEASE_NOTES.md) |

The frozen source, fabrication master, authoritative BOM, and release record control Rev0. The schematic and assembly PDFs are human-readable references.

## 2. Intended role

MakerCard-S is a reference implementation for moving a Core-S-class MCU circuit into a practical prototyping and presentation format. It combines:

- the Core-S G031 Rev0 MCU circuit;
- Core-S contact headers and direct breakout rows;
- connected five-hole breadboard-style strips;
- isolated prototype power rails with optional links;
- a large independent 2.54 mm prototype field;
- a maker-profile area and CraftVia project QR code.

It demonstrates an integration path; it is not a separately socketed Core mounted on a Carrier.

## 3. Physical construction

| Item | Rev0 implementation |
| --- | --- |
| Board outline | 91.0 mm x 55.0 mm rounded rectangle |
| Corner radius | 3.0 mm |
| Copper layers | 4 |
| Mounting holes | Four 3.2 mm NPTH holes |
| Hole centers | `(5.5,5.0)`, `(85.5,5.0)`, `(5.5,50.0)`, `(85.5,50.0)` mm |
| Grid | 2.54 mm prototype and breakout grid |
| Bottom-side SMT | None |

The large board may overhang a Core-S host. Electrical mapping does not by itself prove mechanical compatibility with Station-S; mating and clearance are validation items.

## 4. Integrated Core-S G031 circuit

The MCU, power, SWD/BOOT, reset, and clock-option circuit matches the Core-S G031 v1 Rev0 design intent.

| Function | Rev0 configuration |
| --- | --- |
| Supply | 3.3 V on `K-SW`; GND on `K-NW` and `K-NE` |
| MCU | STM32G031K8T6, LQFP32 |
| Debug | N1 NRST, N2 SWDIO, N3 SWCLK, N4 boot request |
| Default clock | HSI16; external 8 MHz oscillator network DNP |
| Decoupling | C1/C3=100 nF, C2=4.7 uF |
| Supply link | R7=0 ohm fitted |
| PC14/PC15 links | R3 and R10 fitted |

### Contact map

| Contact | MCU/node | Contact | MCU/node |
| --- | --- | --- | --- |
| N1 | PF2-NRST | E1 | PA0 |
| N2 | PA13/SWDIO | E2 | PA1 |
| N3 | PA14/SWCLK/BOOT0 through R1 | E3 | PA2 |
| N4 | PA14/BOOT0 through R2 | E4 | PA3 |
| N5 | PA11 | E5 | PA4 |
| N6 | PA12 | E6 | PA5 |
| N7 | PB8 | E7 | PA6 |
| N8 | Breakout net only; no MCU connection | E8 | PA7 |
| S1 | PA8 | W1 | PA9 / recommended USART1_TX |
| S2 | PB0 | W2 | PA10 / recommended USART1_RX |
| S3 | PB1 | W3 | PB6 / recommended I2C1_SCL |
| S4 | PC6 | W4 | PB7 / recommended I2C1_SDA |
| S5 | PB2 | W5 | PB3 / recommended SPI1_SCK |
| S6 | PB9 | W6 | PB4 / recommended SPI1_MISO |
| S7 | PC14 through R3 | W7 | PB5 / recommended SPI1_MOSI |
| S8 | PC15 through R10 | W8 | PA15 / recommended SPI1_NSS |

N3 and N4 are not independent MCU pins. Isolate SWCLK before actively driving the boot request.

## 5. Core contact and breakout hardware

J1-J4 are the N, E, S, and W 1x8 headers. J5-J7 are the three key/power headers. They are in the authoritative BOM but normally require through-hole service or manual assembly.

J8-J11 duplicate the four eight-contact edges as unpopulated breakout rows. J12-J14 duplicate the three key contacts. The breakout designators are excluded from the authoritative BOM.

N8 is routed between its header and breakout position, but it has no MCU connection in the integrated G031 circuit.

## 6. Prototype areas and rails

### 6.1 Breadboard-style strips

BB1-BB12 are connected five-hole strips. Each strip is one electrical net; different strips are isolated from one another. They are copper features/footprints, not populated connectors.

### 6.2 Power rails

BBP1-BBP4 are four independent eight-hole rails:

| Rail | Optional link | Link source | Rev0 default |
| --- | --- | --- | --- |
| `BB_PWR_A` | SJ1 | +3V3 | Open / DNP |
| `BB_PWR_B` | SJ2 | GND | Open / DNP |
| `BB_PWR_C` | SJ3 | +3V3 | Open / DNP |
| `BB_PWR_D` | SJ4 | GND | Open / DNP |

Do not bridge SJ1-SJ4 until continuity, intended polarity, and the connected load have been checked. The rail labels describe the optional source, not the as-fabricated state.

### 6.3 Universal field

The universal 2.54 mm field consists of individually isolated plated through holes unless the user adds wiring. The first article must verify isolation throughout the field and from the four rails.

## 7. Assembly population

The authoritative BOM contains the same 16 integrated-Core designators as Core-S G031 Rev0: `C1`, `C2`, `C3`, `J1-J7`, `R1`, `R2`, `R3`, `R7`, `R10`, and `U1`.

The following are DNP or excluded:

- clock/reset options: `C4`, `R4`, `R5`, `R6`, `R8`, `R9`, `R11`, `X1`;
- bare test points: `TP1-TP5`;
- breakout/prototype features: `BB1-BB12`, `BBP1-BBP4`, `J8-J14`, `SJ1-SJ4`.

The unfiltered CPL contains 56 designators and must be filtered against the 16 populated BOM designators. J1-J7 are through-hole parts and are not normally installed by standard SMT service.

## 8. Maker-profile and brand area

The back-side maker-profile area is intended to be customized before a third party manufactures or distributes a personalized board. Replace the example person's profile information with the distributor's own authorized information.

The reference QR code points to `https://craftvia.org/mc` and may be retained. Open-hardware permission does not grant permission to impersonate a person, imply endorsement, or present a modified or third-party-manufactured board as an official CraftVia product. Follow the repository Trademark Policy.

## 9. Known Rev0 limitations

- No assembled or powered sample has been tested.
- Core-S/Station-S mechanical mating with the larger board outline is unverified.
- J1-J7 header dimensions and assembly method require confirmation.
- Prototype strip continuity, universal-field isolation, and SJ default-open state are unverified.
- Current consumption, boot behavior, SWD, and all breakout mappings are unverified.
- The optional oscillator network is unverified and DNP.
- The schematic PDF retains historical draft labels.

## 10. Verification

Use [`verification-plan-v1-rev0.md`](verification-plan-v1-rev0.md). Retain photographs of both sides before and after assembly because the maker-profile artwork, QR code, hole field, and rail markings are part of the Rev0 identification and usability checks.

