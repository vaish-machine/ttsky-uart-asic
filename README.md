
# SKY130 UART ASIC submission candidate

This directory contains a frozen physical submission candidate
for tt_um_vaish_uart using the Tiny Tapeout custom GDS workflow.

The GDS is the locally verified Stage 3Y ECO layout.
The RTL and mapped netlist must remain consistent with the
saved formal-equivalence evidence.

## Local verification

- Local DRC and LVS passed.
- Nine timing corners were analyzed successfully.
- Unbounded reset-conditioned RTL equivalence passed.
- Power and ground connectivity independently passed.
- Proposed external macro-interface timing passed.

## Not yet completed

- Provider-specific precheck.
- Provider acceptance of this exact layout and LEF.
- Board-level timing and controller compatibility.
- Quantitative CDC reliability assessment.
- Fabrication submission and payment.
- Post-fabrication silicon validation.

The GitHub Actions workflows are deliberately configured for
manual execution. No physical hardening or layout regeneration
is performed by the custom GDS workflow.

The provisional action release is ttsky26d. Confirm the selected
shuttle and metadata before publishing or submitting.
