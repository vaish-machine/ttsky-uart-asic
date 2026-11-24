
# How it works

This project implements an 8-bit UART transmitter and receiver
with one start bit, eight data bits, no parity and one stop bit.

The transmitter accepts parallel data on ui_in[7:0].
The TX_VALID signal initiates transmission when TX_READY is high.
The serial output is available on uio_out[3].

The receiver accepts asynchronous serial data on uio_in[1].
A two-stage synchronizer feeds the UART receive logic.
Received data appears on uo_out[7:0], together with RX_VALID
and RX_FRAMING_ERROR indications.

The UART uses 16 clock cycles per serial bit.
At a 10 MHz input clock the baud rate is 625000.

Reset is synchronous and active low.

## Pinout

| Interface | Function |
| --- | --- |
| ui_in[7:0] | Parallel TX data |
| uo_out[7:0] | Parallel RX data |
| uio[0] | TX_VALID input |
| uio[1] | Serial RX input |
| uio[2] | TX_READY output |
| uio[3] | Serial TX output |
| uio[4] | RX_VALID output |
| uio[5] | RX_FRAMING_ERROR output |
| uio[6:7] | Unused inputs |
| clk | Project clock |
| rst_n | Synchronous active-low reset |

# How to test

1. Select the UART project using the Tiny Tapeout controller.
2. Supply a stable clock. The locally analyzed target is 10 MHz.
3. Hold reset low across multiple rising clock edges and release
   it synchronously. Keep the serial RX input idle high.
4. Confirm that TX_READY is high and serial TX is idle high.
5. Present a test byte on the parallel input interface.
6. Assert TX_VALID for one valid rising edge while TX_READY is high.
7. Capture serial TX and decode an 8N1 frame at clock_hz / 16 baud.
8. Drive a valid serial RX frame and confirm received parallel data
   and the RX_VALID indication.
9. Repeat using all-zero, all-one, alternating and random data.
10. Send a deliberately invalid stop bit and verify framing-error
    detection.
11. Exercise reset during TX and RX, then confirm recovery.
12. Exercise TX-to-RX loopback and consecutive transfers.

These are planned post-fabrication tests, not hardware results.

# External hardware

Use a Tiny Tapeout-compatible controller and voltage-compatible
serial source or analyzer.

The parallel transmit data, TX_VALID and reset inputs must
meet a synchronous timing contract at the project macro boundary.

A proposed contract with input arrival at 40 to 60 ns after
a rising edge and output sampling at the falling edge was
validated locally at nine timing corners.

Actual controller, pad and multiplexer timing remains to be
validated. The asynchronous RX input is synchronized, but a
quantitative metastability MTBF is not claimed.

Do not connect RS-232 voltage levels directly to CMOS GPIO.
