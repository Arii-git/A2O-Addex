# Manual OpenROAD flow: gcd on Nangate45

Run by hand, one stage at a time, then repeated as a single script (flow.tcl).
Stages: setup, floorplan, tapcells, PDN, pins, global placement, resizer,
detailed placement, CTS, global route, detailed route, filler, signoff.

## Files
- gcd_netlist.v, gcd_1ns.sdc, gcd_pdn.tcl: inputs
- flow.tcl: stages 0 to 12 as one script
- flow.log: full log of the scripted run
- results/: gcd_final.def, gcd_final.odb, gcd_final.spef
- reports/drc.rpt: empty, meaning no DRC violations
- gcdlay*: layout saved from the OpenROAD GUI

## Verified results (from flow.log)
- Detailed route finished with 0 DRC violations
- Antenna check: 0 net and 0 pin violations
- Clock skew about 0.00 ns, 34 clock sinks
- Power 4.78 mW total
- Design area 567 um^2 at 45% utilisation
- Global route: no congestion overflow, wirelength 5075 um

## Limits
- Timing is not closed: WNS -0.50 ns, TNS -19.82 ns
- gcd_1ns.sdc is named for 1 ns but sets a 0.25 ns clock period
- This run is separate from the earlier ORFS gcd run, so numbers are not directly comparable
- reports/congestion.rpt was not produced
- def_to_gds.py is not in the container and write_gds is not in this OpenROAD build
- The layout is gcd, not the A2O core

## Screenshots
- 03_GCD_openroad_lay.png: OpenROAD GUI showing the routed gcd layout loaded from results/gcd_final.odb

## Utilisation and clock experiments

All three runs use the same netlist (230 cells, 398 um2). Each run is in its own folder.

| Folder | Utilisation setting | Clock | Metal4 pitch | Result |
|---|---|---|---|---|
| manual_flow | 30 | 0.25 ns | 56 um | 567 um2 at 45%, WNS -0.50 ns, TNS -19.82 ns, hold +0.07 ns, DRC 0 |
| manual_flow_u60 | 60 | 0.25 ns | 28 um | stopped at detailed placement (DPL-0033), no sign-off reports |
| manual_flow_u60_1ns | 60 | 1.0 ns | 28 um | 422 um2 at 69%, setup +0.11 ns, hold +0.10 ns, DRC 0 |

Notes
- The 60% die is 29.755 um square, so the 56 um metal4 pitch no longer fits (PDN-0185). I halved it to 28 um.
- At 60% and 0.25 ns, timing-driven placement added about 182 um2 of buffers, which left too little free area (placed area 652 um2 against 601 um2 free). The legaliser did not converge.
- manual_flow_u60_1ns changes three things at once (utilisation, clock period, metal4 pitch), so it does not isolate the effect of utilisation alone.
- Reports are written to reports/ by flow.tcl on every run. drc.rpt is empty because detailed routing found no violations. congestion.rpt is not written in these runs.
- manual_flow_u60 keeps its flow.log as the record of the failed run. Its flow.tcl was run before the report block was added.
