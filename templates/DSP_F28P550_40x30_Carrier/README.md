# DSP F28P550 40x30 carrier interface

## Use
Place module_interfaces:DSP_F28P550_40x30_J1 and DSP_F28P550_40x30_J2 together, or reuse this schematic as a hierarchical sheet. These are two real Samtec sockets, not a duplicate MCU circuit. Pin names describe the attached DSP module. All used connector pins are passive; this library does not replace system-level direction/voltage checks.

Append/copy the grouped PCB template for the matching placement. After schematic annotation, associate the two footprints with their corresponding symbols using KiCad. Do not import the review PCB into a product. The template intentionally has no Edge.Cuts, copper routing or board-size definition. Local labels in a reused sheet need hierarchical ports/wires or intentional parent net connections when integrating another design.

## Physical placement
Top view of carrier, module above carrier; module top side faces the viewer. DSP headers are on its bottom. Both carrier sockets are F.Cu, rotation 180 degrees; do NOT mirror them manually. J1 center=(120,104.5), J2=(120,125.5) mm. Separation=21 mm. Module envelope=(100,100) to (140,130) mm. Contact pitch=1.27 mm, 2x22 each. Pin 1 is at the left end, on the upper contact row in this carrier view. Copper pad centers differ between header and socket because their lead lengths differ; contact axes, not pad centroids, determine mating.

The pair and envelope are grouped and locked. Move the group together; preserve rotation and spacing. Outline is Dwgs.User only, not a carrier cutout. No mounting holes or mechanical retention are specified. Full module underside component clearance still needs checking; do not assume free placement under the module. Nominal mating board spacing from the prior connector review is 5.207 mm, not a guaranteed spacer specification.

## Electrical / BOM
Supply the module with 5V. Digital interfaces including PWM/SCI/SPI/I2C use 3.3V logic. ADC inputs are 0-3V. +3V3_SENSE is a high-impedance monitor, not an external power supply. CAN H/L pins already pass through module transceivers. Keep NC pins unused. Include both sockets and all supply/ground contacts.

Native BOM has two physical socket references. Grouping by Value/Manufacturer/PartNumber/Footprint yields quantity 2; descriptions differ to identify J1/J2 so stricter grouping may retain two rows. Cost is unknown/blank. Mechanical envelope is excluded from BOM/position output; the DSP module itself is a separate assembly, not included in this socket BOM.

Pinmap was extracted from DSP native netlist on 2026-09-28. This interface matches the current 40x30 layout, which still has unfinished routing. Changes to module connector position or pinmap require a new compatibility review. Existing CLP footprint/nominal STEP envelope are reused without copying; STEP is simplified drawing-derived geometry, not manufacturer CAD.

## Sources
Samtec CLP drawing: https://suddendocs.samtec.com/prints/clp-1xx-xx-xxx-d-xx-xx-xx-mkt.pdf
Samtec FTSH drawing: https://suddendocs.samtec.com/prints/ftsh-1xx-xx-xxx-dv-xxx-xxx-x-xx-mkt.pdf
Module source: dsp_board/kicad/dsp_board.kicad_pcb and native schematic netlist.
