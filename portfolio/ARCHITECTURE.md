# SKY130 UART architecture

```mermaid
flowchart LR
    UI[Parallel TX data] --> TX[TX state machine]
    VALID[TX_VALID] --> TX
    TX --> SHIFT[8-bit shift register]
    SHIFT --> SERIAL[TX_SERIAL]
    TX --> READY[TX_READY]
    RXI[Asynchronous RX_SERIAL] --> META[First synchronizer]
    META --> SYNC[Second synchronizer]
    SYNC --> RX[RX state machine]
    RX --> SAMPLE[Mid-bit sampling]
    SAMPLE --> DATA[Parallel RX data]
    RX --> RV[RX_VALID]
    RX --> FE[RX_FRAMING_ERROR]
    CLK[Clock and synchronous reset] --> TX
    CLK --> RX
    CLK --> META
    CLK --> SYNC
```

## Design characteristics

- 8N1 UART, LSB first.
- 16 clocks per bit.
- 625000 baud at 10 MHz.
- Synchronous active-low reset.
- Two-flop RX synchronizer.
- Stop-bit validation and framing-error reporting.
- No FIFO or hardware flow control.

The synchronizer is not a quantitative metastability
reliability guarantee.

See [provider pinout](../docs/info.md).
