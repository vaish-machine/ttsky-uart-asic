# Reproducibility

## Verify the repository artifacts

```bash
sha256sum --check RELEASE_SHA256SUMS.txt
```

## Reproduce the functional simulation

Install Icarus Verilog and optionally GTKWave.

From the repository root:

```bash
mkdir -p /tmp/uart-asic-sim

iverilog -g2012 -s tb_tt_um_vaish_uart \
  -o /tmp/uart-asic-sim/wrapper.vvp \
  src/uart_tx.sv src/uart_rx.sv \
  src/uart_core.sv src/tt_um_vaish_uart.v \
  portfolio/simulation/tb_tt_um_vaish_uart.sv

cd /tmp/uart-asic-sim
vvp wrapper.vvp
gtkwave uart-wrapper.vcd
```

## Reproduce the physical implementation methodology

Historical scripts, testbenches, formal checks,
constraints and physical experiments are preserved at:

https://github.com/vaish-machine/rtl-dv-lab/tree/e2c612a965548cc12c411957594874496a0d8a8e/asic_tapeout

Use LibreLane 3.0.17, the recorded SKY130A PDK
revision, SKY130 HD cells, 3x2 Tiny Tapeout
footprint and 100 ns target clock period.

See the archived final SDC and toolchain provenance.

The historical physical ECO depends on intermediate
artifacts and the original implementation environment.
A portable clean-room rebuild with an identical GDS
hash has not yet been demonstrated.

## Official verification

Manufacturing commit: `5ae3cfc4481abf22cda836ca8179ee21ce64b305`

Provider workflow: https://github.com/vaish-machine/ttsky-uart-asic/actions/runs/45433759245

The official custom-GDS workflow validates and
packages existing physical design artifacts.
It does not regenerate the layout.
