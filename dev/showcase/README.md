# addex: showcase

Added the `addex` instruction to A2O: decode in `xu0_dec.v` and `xu1_dec.v`, OV carry-in in `xu_alu.v`, and a 19-case ALU testbench. The testbench result is `*** OVERALL: PASS` (see `addex_test_result.txt`).

## Test result
![Test result](02_addex_test_pass.png)

## Waveform
![GTKWave waveform](03_gtkwave_waveform.png)

## Layout (gcd example flow)
![KLayout layout](01_klayout_layout.png)
