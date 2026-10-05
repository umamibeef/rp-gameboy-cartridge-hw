# Changelog

Revision history of the Croco Cartridge hardware, newest first. Dates are when a revision was published in this
repository.

For a new revision: add a section for it at the top, set the `VERSION` text variable in the KiCad project to match
(Schematic Setup, Text Variables), regenerate the fab outputs, and tag the commit `vX.Y`. The revision printed on the
board and in the title blocks comes from `VERSION`.

## V2.2 (2026-10-05)

KiCad project: `KiCad/V2/GameboyCartridgeV2`

Moved from KiCad 9 to KiCad 10. Most of the changes below come from that move: footprints updated from the KiCad 10
libraries, and fixes for issues found by the design rule check in KiCad 10.

- Added: Croco pixel art on the back of the board: exposed gold (ENIG) skin with white silkscreen eyes, belly and
  outline. A short stretch of the RTC_CS trace and the edges of two GND vias show inside the gold.
- Added: Title blocks on the schematic and board. The board revision and date come from the `VERSION` variable instead
  of hand-edited silkscreen text.
- Changed: Footprints refreshed from the KiCad 10 libraries. Copper changes: slightly larger pads for the SPI flash (U2,
  now `SOIC-8_5.3x5.3mm`), shorter pads for the RTC (U5, MSOP-10), and rounded pads on Y1, Y2 and D1. The USB-C (J1) and
  microSD (J2) shield pins are now called `SH` in both schematic and footprints.
- Changed: Minimum copper clearance is now 0.15 mm (6 mil), which the RP2350's 0.4 mm pitch pads need.
- Changed: Ten GND vias enlarged from 0.45 mm to 0.5 mm diameter.
- Changed: The OSHW logo and the image logo on the back were moved.
- Changed: KiCad project renamed from `GameboyCartridgeV2.1` to `GameboyCartridgeV2`, so file names no longer carry the
  revision.
- Changed: Plot settings set up for PCBWay: Protel filename extensions off, Gerber job file and netlist attributes on,
  and solder mask no longer subtracted from silkscreen. Gerber and drill files are written to `gerbers/v<VERSION>/`, and
  the V2.1 files moved to `gerbers/v2.1/`.
- Fixed: The soft-button contacts (SW1) and the back-side strips of the edge connector are now real pads on their nets,
  so DRC checks them. The copper itself is unchanged.
- Fixed: Removed two unconnected vias from the microSD card-detect pads. They served no purpose and could draw solder
  away from the joints.
- Fixed: Silkscreen clean-up. Edge-connector pin labels raised to 0.8 mm text height, overlapping reference designators
  moved, test-point labels hidden, and the debug header (J3) outline no longer runs off the board edge.
- Fixed: ERC is clean (was 63 items); the netlist is unchanged. Power flags on GND, VBUS, +1V1, +BATT, VREG_AVDD, VCC
  and +5V. No-connect markers on the pins left open on purpose (U4 RESET# and CK#, J5 WR and SND). J5's pins set to
  passive. The HyperRAM data bus is labelled `HYPERRAM_DQ[0..7]`.
- Changed: Symbols whose source libraries are missing or were redrawn in KiCad 10 (U1, U2, U3, U4, JP1, Y1, J5) are now
  kept in the project's CrocoV2 symbol library, unchanged. BT1 uses the KiCad 10 Battery_Cell symbol, and Q1 now links
  to `Transistor_FET:Q_NMOS_GSD`.
- Fixed: Y2 3D model. The KiCad 10 library points Y2 at a model that is not installed, so it now uses the MicroCrystal
  CC8V model of the same 2.0 x 1.2 mm size.
- Fixed: The HyperRAM (U4) description and datasheet link now describe the fitted part (Winbond W956A8MBYA5I, 64 Mbit,
  3.0 V). The part number and LCSC code were already correct.
- Fixed: DRC is clean. The four USB-C (J1) pad to alignment-peg hole clearances of 0.19 mm come from the connector's own
  land pattern and are excluded.

## V2.1 (2025-07-16)

KiCad project: `KiCad/V2/GameboyCartridgeV2.1` (renamed in V2.2)

Redesign around the RP2350.

- Changed: RP2350B (QFN-80) replaces the RP2040, with the core supply from its built-in switching regulator (3.3 uH
  inductor).
- Added: 64 Mbit HyperRAM (Winbond W956A8MBYA5I, TFBGA-24).
- Added: microSD card slot (Hirose DM3D-SF) on SPI.
- Added: Battery-backed real-time clock (MCP79510) with a 32.768 kHz crystal and a 12 mm coin cell holder (Keystone
  3000), sharing the SPI bus.
- Added: TVS protection (five AOZ8809DI-05 arrays) on the cartridge bus lines.
- Added: User button (SW1) made from interleaved contact pads.
- Added: Four test points.
- Changed: Level shifting reduced to one TXB0108 on the data bus. The address and control lines now go straight to the
  RP2350 (V1.3 used a TXB0102 and three TXB0108).
- Changed: SPI flash reduced from 128 Mbit (W25Q128JV) to 16 Mbit (W25Q16JV).
- Changed: Reset button is now a PTS810 (was a TL3342).
- Changed: Tag-Connect TC2030 debug footprint is now the no-legs version.
- Removed: Fiducials.

## V1.3 (2025-11-29)

KiCad project: `KiCad/V1/GameboyCartridgeV1.3` (RP2040)

- Added: 5.1 kOhm pull-downs on the USB-C CC pins, so the cartridge gets power from USB-C to USB-C cables.
- Added: USB ESD protection (USBLC6-2P6) on D+ and D-, and a polyfuse on VBUS.
- Changed: 3.3 V regulator is now an AP2112K-3.3 (was an MCP1825T-3302E with a power-good output).
- Changed: The gate of the Game Boy reset transistor (Q1) is now pulled up to 3.3 V instead of down to GND.

## V1.1 (2023-11-03)

KiCad project: `KiCad/V1/GameboyCartridgeV1.1` (RP2040)

First published revision: RP2040 with 128 Mbit QSPI flash (W25Q128JV), TXB0102 and TXB0108 level shifters between the
Game Boy cartridge bus and the RP2040, an MCP1825T 3.3 V regulator, and USB-C.
