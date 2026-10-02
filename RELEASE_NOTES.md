# CraftVia MakerCard-S v1 Rev0 Hardware Release

## Release status

- Revision: Rev0
- Release date: 2026-09-30
- Status: FROZEN for initial prototype PCB fabrication
- Intended use: Rev0 engineering prototype; not a general-production release
- CAD: EasyEDA v3.2.149.88089769
- Authoritative editable source: `source/CraftVia-MakerCard-S-v1-Rev0.eprj2`
- Authoritative PCB fabrication upload: `fabrication/Gerber_PCB1_2026-09-30.zip`

This release was checked against the project opened in EasyEDA on 2026-09-30. The project, schematic-page and PCB document UUIDs correspond to the frozen source package. The exported BOM, CPL, schematic PDF and PCB PDF were also checked against the live design.

## Release decision

The package is acceptable for an initial Rev0 PCB fabrication order, subject to the ordering and assembly conditions below.

- PCB strict DRC: PASS, 0 errors
- Schematic DRC: Fatal 0, Error 0, Warning 1
- The remaining schematic warning is non-fatal. EasyEDA's bridge result exposes the warning count but not its detailed UI message; it does not correspond to a PCB connectivity or clearance error.
- Gerber ZIP integrity: PASS
- Board: 91.0 mm x 55.0 mm, 3.0 mm corner radius, 4 copper layers
- Mounting holes: four 3.2 mm NPTH holes
- PCB contents checked: 56 component/feature footprints, 482 routed line primitives, 36 vias, 2 copper pours and 59 nets

## Release contents

| Path | Purpose |
| --- | --- |
| `source/CraftVia-MakerCard-S-v1-Rev0.eprj2` | Authoritative editable EasyEDA project |
| `source/ProPrj_CraftVia-MakerCard-S-v1-Rev0_2026-09-30.epro` | Portable EasyEDA project archive |
| `fabrication/Gerber_PCB1_2026-09-30.zip` | Canonical PCB fabrication package |
| `assembly/BOM_Board1_PCB1_2026-09-30.csv` | Authoritative populated-part BOM |
| `assembly/PickAndPlace_PCB1_2026_09_30.csv` | Full-board coordinate export; filter before turnkey assembly upload |
| `assembly/PCB_PCB1_2026-09-30.pdf` | PCB, BOM and assembly/layer reference export |
| `docs/SCH_MakerCard-S-Rev0_2026-09-30.pdf` | Human-readable schematic reference |
| `SHA256SUMS.txt` | SHA-256 integrity manifest |

## PCB fabrication package

Upload `fabrication/Gerber_PCB1_2026-09-30.zip` directly to the PCB manufacturer. Do not edit or recompress its contents.

The ZIP contains:

- Top, Inner 1, Inner 2 and Bottom copper
- Top and Bottom solder mask
- Top and Bottom silkscreen
- Top paste mask
- Top assembly and document layers
- Board outline
- NPTH, plated component-hole and plated-via drill data

No Bottom paste file is expected because there are no Bottom-side SMT parts.

Before submitting the order, load the exact ZIP into JLCDFM or the board manufacturer's Gerber preview and confirm:

- 91.0 mm x 55.0 mm rounded-rectangle outline with 3.0 mm corner radius
- four copper layers
- four 3.2 mm mounting holes are recognized as NPTH at `(5.5, 5.0)`, `(85.5, 5.0)`, `(5.5, 50.0)` and `(85.5, 50.0)` mm
- the large prototype-field and breakout-hole arrays are recognized as plated through holes
- Top/Bottom silkscreen, QR code, solder-mask openings and board text are legible and correctly oriented
- no drill offset, duplicate-drill or unexpected slot warning is reported

The ZIP contains both `Drill_PTH_Through.DRL` and `Drill_PTH_Through_Via.DRL`. Use the complete ZIP and verify the manufacturer's preview rather than uploading selected drill files manually.

The manufacturer's order-time stack-up selection is authoritative. Use a normal 4-layer FR-4 prototype stack unless a later release specifies a controlled stack-up.

## Assembly population

The BOM contains 15 lines and 16 populated designators:

`C1, C2, C3, J1, J2, J3, J4, J5, J6, J7, R1, R2, R3, R7, R10, U1`

The following electrical option parts are intentionally DNP in Rev0:

`C4, R4, R5, R6, R8, R9, R11, X1`

The following test points are PCB features and are not populated BOM items:

`TP1, TP2, TP3, TP4, TP5`

The following breakout, prototype-area and rail-link footprints are intentionally excluded from the assembly BOM:

`BB1-BB12, BBP1-BBP4, J8-J14, SJ1-SJ4`

### Pick-and-place warning

`assembly/PickAndPlace_PCB1_2026_09_30.csv` is an unfiltered 56-designator board export. It includes 40 DNP/test-point/prototype-feature rows in addition to the 16 BOM designators.

- For PCB fabrication only, this does not affect the Gerber order.
- For turnkey assembly, do not upload the CPL unchanged without checking the import result.
- Filter the CPL to the 16 authoritative BOM designators, or verify that the assembler uses the BOM/CPL designator intersection and marks every other row DNP.
- BOM population status takes precedence over the unfiltered CPL and the visual red-X markings in the assembly PDF.

J1-J7 are through-hole pin headers. Standard SMT assembly does not normally mount them. Select an explicit THT service or plan manual post-assembly insertion. J1-J4 use supplier reference `C49256`; confirm square-pin size, mating-pin length and insulator height against the intended Core-S/Station-S mechanical stack before purchase. J5-J7 use `C81276` with nominal 6 mm mating pin and 2.5 mm insulator height.

## Datasheet and electrical configuration

- U1: STM32G031K8T6, LQFP32, 7 mm x 7 mm, 0.8 mm pitch; package and pin mapping match the frozen footprint
- Input supply: 3.3 V
- C1/C3: 100 nF and C2: 4.7 µF; consistent with the STM32G031 supply-decoupling guidance
- R7: fitted, 0 ohm; supplies `VDD_CORE`
- R3: fitted, 0 ohm GPIO link
- R10: fitted, 0 ohm
- External 8 MHz oscillator option: DNP; internal HSI is the default clock source
- ASEDVN option footprint pin mapping: 1=`OSC_EN`, 2=GND, 3=`OSC_OUT`, 4=`VDD_OSC`; consistent with the oscillator datasheet
- PA14 is shared by SWCLK and BOOT0; firmware and Station-S selection/isolation must match the assembled link state
- Prototype power rails remain isolated unless the corresponding `SJ1-SJ4` link is intentionally bridged

## Known documentation notes

The schematic PDF still contains the historical labels `SCHEMATIC DRAFT` and `DESIGN NOTES - NOT RELEASED FOR FABRICATION`. For this frozen folder, the schematic PDF is a circuit-review reference only; it is not a manufacturing master. This README and the SHA-256 manifest define the Rev0 release status, and the Gerber ZIP is the sole PCB fabrication master.

The assembly PDF is an EasyEDA multi-page reference export. Its red-X overlays indicate excluded/DNP features and are not a substitute for the BOM. The BOM CSV is authoritative for population.

For a later revision or a cleaned documentation-only repack, remove the historical draft labels and regenerate the schematic PDF from an otherwise unchanged source.

## First-article checks

1. Inspect the 91.0 mm x 55.0 mm outline, four corner holes, QR code and both-side text before accepting manufacture.
2. Check continuity and isolation of every prototype-field row and the four power rails before fitting components.
3. Confirm `SJ1-SJ4` are open and that the rail pairs are isolated in the as-fabricated state.
4. Inspect orientation and continuity of J1-J7 before mating another CraftVia board.
5. Power from a current-limited 3.3 V supply and confirm no short between `VDD_CORE` and GND.
6. Measure 3.3 V at U1 VDD/VDDA and verify the expected idle current.
7. Verify SWD through PA13/SWDIO, PA14/SWCLK and PF2/NRST, then verify BOOT behavior on the shared PA14 network.
8. Exercise the N/E/S/W breakout mappings and the breadboard/prototype-area nets with a continuity fixture.
9. Leave X1 and its option network DNP for the initial internal-HSI bring-up.

## Change control and integrity

Verify every file listed in `SHA256SUMS.txt` before ordering. After this release is used for an order, do not overwrite any file captured by the `v1-rev0` tag or its GitHub Release assets. Electrical, footprint, routing, artwork, silkscreen or population changes require a new revision. Documentation-only corrections must be made in a later commit without moving or replacing the tag.
