# CraftVia MakerCard-S v1 Rev0 Verification Plan

> Status: Draft verification plan for unverified Rev0 hardware. No successful operation is implied until results are published.

## 1. Test record

| Field | Record |
| --- | --- |
| Board serial or sample ID |  |
| PCB marking | `CraftVia MakerCard-S Rev0` |
| Assembly source and date |  |
| BOM deviations or rework |  |
| Test date and operator |  |
| Instruments and calibration state |  |
| Power-supply current limit |  |
| Firmware commit/build |  |
| Station or fixture revision |  |
| Maker-profile customization state |  |

Each test result must be **Pass**, **Fail**, **Blocked**, or **Not run**, with measurements and evidence.

## 2. Safety and stop conditions

- Keep SJ1-SJ4 open for initial inspection and Core bring-up.
- Insert into any host only while unpowered.
- Use a current-limited 3.3 V supply for first power.
- Stop for unexpected current, rail collapse, heating, smoke, odor, or a short in the prototype field.
- Treat every prototype rail as unpowered until it has been measured.

## 3. Required equipment

- microscope or magnifier;
- digital multimeter and continuity fixture;
- current-limited 3.3 V supply;
- SWD probe or separately validated Station-S;
- oscilloscope or logic analyzer;
- fixtures or leads for every Core contact, breakout row, strip, rail, and universal pad under test;
- basic bring-up firmware.

## 4. Tests

| ID | Test | Method | Pass criteria | Evidence |
| --- | --- | --- | --- | --- |
| `MCS-R0-001` | Release identity | Verify board marking, source release, BOM, Gerber, and SHA-256 manifest. | All identifiers indicate v1 Rev0 and files match the manifest. | Photos and checksum log |
| `MCS-R0-002` | Artwork and visual inspection | Inspect both sides, QR code, profile text, soldering, polarity, DNP sites, and hole plating. | Artwork is legible and authorized; population matches the BOM; no visible defect. | High-resolution photos |
| `MCS-R0-003` | Outline and mounting holes | Measure 91 x 55 mm outline, 3 mm corner radius, and four 3.2 mm NPTH holes. | Dimensions and hole positions match the release specification within fabrication tolerance. | Measurement sheet |
| `MCS-R0-004` | Header mechanics | Inspect J1-J7 orientation, pin dimensions, projection, alignment, and clearance. | Headers match the intended orientation and mate without collision or excessive force. | Photos and measurements |
| `MCS-R0-005` | Unpowered supply checks | Measure `+3V3`/`VDD_CORE` to GND and verify R7 continuity. | No short; R7 supply path is continuous. | Resistance readings |
| `MCS-R0-006` | Core contact map | Check J1-J7 against the published N/E/S/W/key map, including N8 isolation from the MCU. | All mapped contacts are correct; N8 reaches only its breakout net; no adjacent short. | Contact matrix |
| `MCS-R0-007` | Breakout duplication | Check J8-J14 against J1-J7. | Every breakout contact reaches only its corresponding Core contact. | Continuity matrix |
| `MCS-R0-008` | Five-hole strips | Check each BB1-BB12 group internally and between groups. | All five holes in each strip are connected; different strips are isolated. | Strip matrix |
| `MCS-R0-009` | Power rails and links | Check BBP1-BBP4 and SJ1-SJ4 with links open. | Each eight-hole rail is internally connected, rails are mutually isolated, and no rail is connected to +3V3 or GND by default. | Rail matrix |
| `MCS-R0-010` | Universal field | Sample or fixture-test all independent universal pads against neighbors, rails, GND, and +3V3. | Pads intended to be independent are isolated; no unintended short. | Fixture log |
| `MCS-R0-011` | First power | Apply 3.3 V through the intended input with a conservative current limit and no prototype load. | Stable rail, no current-limit event, no abnormal heating. | Current trace and thermal notes |
| `MCS-R0-012` | Rail voltage | Measure `+3V3`, `VDD_CORE`, and U1 VDD/VDDA. | Powered nodes remain within 3.135-3.465 V and are mutually consistent. | Voltage table |
| `MCS-R0-013` | SWD and reset | Detect, erase, program, debug, reset, and test connect-under-reset through N1-N3 or test points. | All operations succeed repeatedly with the correct device ID. | Tool log |
| `MCS-R0-014` | Boot selection | Isolate SWCLK and perform the N1/N4 boot sequence. | Normal and system-memory boot are selectable without contention. | Option-byte and boot log |
| `MCS-R0-015` | Standard communications | Test W1/W2 UART, W3/W4 I2C, and W5-W8 SPI. | Bidirectional transfers pass and contact mapping is correct. | Bus captures |
| `MCS-R0-016` | ADC, timer, and GPIO | Exercise E1-E8, S1-S8, and N5-N7 within device limits. | Each contact maps correctly; ADC and waveform results are recorded. | Measurement log |
| `MCS-R0-017` | Optional rail links | After default-open tests pass, bridge one SJ at a time using a controlled test configuration. | The selected rail alone connects to the intended +3V3 or GND source; removing the bridge restores isolation. | Before/after matrix |
| `MCS-R0-018` | Station mating | If mechanically safe, mate to Station-S unpowered and repeat power, SWD, reset, and UART checks. | No collision, acceptable seating, correct orientation, and equivalent electrical operation. | Photos and repeat log |
| `MCS-R0-019` | QR/profile usability | Scan the QR code and review profile text at normal viewing distance. | QR resolves to the intended CraftVia URL and no unauthorized personal information is present. | Scan result and photo |
| `MCS-R0-020` | Representative prototype | Build a small documented circuit using one strip, one independent pad area, and optionally one linked rail. | The circuit operates as designed without revealing unintended connections. | Build notes and photos |

## 5. Verification decision

Rev0 may be marked **Verified** only when the integrated Core, all breakout structures, default-isolated rails, universal field, and intended mechanical use pass. Optional SJ-linked configurations and Station mating may be reported as separately verified configurations. Any PCB or population correction requires a new revision.
