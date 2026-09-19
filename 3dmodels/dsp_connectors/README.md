# dsp_connectors library

Prepared 2026-09-11 for the DSP module. Two exact part metadata symbols and
two drawing-derived footprints: FTSH-122-03-L-DV and CLP-122-02-L-D.
Electrical symbol graphics derive from KiCad Connector_Generic:Conn_02x22_Odd_Even.
Original KiCad libraries are unmodified; retain the KiCad library attribution
and license/exception when redistributing derived symbols.

Directory layout follows the corporate library convention. Published model
links use `${KICAD_GITHUB}/3dmodels/dsp_connectors/`. The project has a snapshot
of the header symbol. Cost is deliberately empty, not zero.

The STEP files are generated nominal envelopes, NOT Samtec-supplied CAD.
Housing/terminal bounds and all 44 contact axes follow the drawing; molded
reliefs, bent lead radii and internal socket springs are simplified. Red pin 1
in preview images is an annotation and not a claim about actual contact color.
STEP geometry is in mm, Z=0 on the soldering plane, no model transform required.

Manufacturer sources:

- FTSH product drawing Rev FX: https://suddendocs.samtec.com/prints/ftsh-1xx-xx-xxx-dv-xxx-xxx-x-xx-mkt.pdf
- FTSH PCB/stencil Rev H: https://suddendocs.samtec.com/prints/ftsh-1xx-xx-xxx-dv-xxx-footprint.pdf
- CLP product drawing Rev DH: https://suddendocs.samtec.com/prints/clp-1xx-xx-xxx-d-xx-xx-xx-mkt.pdf
- CLP PCB/stencil Rev W: https://suddendocs.samtec.com/prints/clp-1xx-xx-xxx-d-xx-footprint.pdf

Use the drawing's primary inch dimensions converted to mm. Header pads
0.7366 x 2.794 at Y=+/-2.032; socket pads 0.7366 x 1.4732 at Y=+/-1.6129.
Pitch is 1.27 mm. No -A alignment holes, -BE entry holes or latch features.
In component-side views, pin 1 is lower left on the header and lower right on
the socket. Do not mirror a footprint manually to compensate for bottom-side
placement; KiCad handles the layer flip. Check actual board-coordinate mating.

Nominal PCB-surface spacing is 5.207 mm from the two seating heights.
This is not a guaranteed spacer dimension. Stencil recommendation is 0.152 mm
in the source documents; final solder mask, stencil, process and mechanical
retention require assembly review. No production board layout is included.
