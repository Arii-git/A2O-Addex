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
