# haestra
haestra is a from-scratch, corexy 3d printer designed to be compact, clean, quiet, and mechanically satisfying. it's not meant to be flashy or experimental?? well ig?? i mean its just a solid, no-nonsense platform that can actually be built without hunting for obscure hardware that costs 7138$. it's made for fitting right on my bench and keeping it maintainable once it's there. and it looks nice while doing that, so thats even better but in a nutshell, it's made for ... well me. 

<img src=img/haestra-preview.png width=47% height=47%>
<img src=img/haestra-preview2.png width=47% height=47%>
*couldn't for the life of me make the exploded view thing work on freecad fml*

## what it is
- a compact corexy layout with a 220x220x230 print volume  
- mgn12 linear rails across all axes  
- triple z motors with auto leveling support (but no probe needed by default)  
- 2020 aluminum frame, sized to match the bed 
- simple belt routing & approachable wiring (no CANbus unless you want it. no weird MCU splits
- mmmm pink

## why this exists
i didn’t want to build a voron. too expensive, too many moving pieces, too optimized for things i didn’t care about. but i also didn’t want a creality knockoff or an extruded cube with parts jammed into it. so this sits somewhere in the middle??? ig a printer that is pretty cool looking and doesn't eat my (ie hackclub's) wallet alive.

## features
- corexy motion on mgn12 rails
- 220×220×230mm build volume
- triple z-axis with independent motors
- direct drive extruder (e3d mount compatible)
- all printed parts fit on 220×220 bed
- optional side and rear panels for structure or semi-enclosure
- basic klipper firmware config included (no probe / no CAN)

## specifications

| category              | value                        |
|-----------------------|-------------------------------|
| motion system         | corexy (custom belt routing)  |
| x/y axis              | mgn12 linear rails            |
| z axis                | 3x nema17 + t8 leadscrews     |
| extruder mount        | e3d v6 bolt pattern           |
| bed size              | 235 x 235 mm                  |
| usable volume         | 220 x 220 x 230 mm            |
| frame material        | 2020 aluminum extrusion       |
| electronics           | klipper-ready board (tested on BTT Octopus) |
| endstops              | mechanical only               |
| overall footprint     | 365 x 365 x 400 mm            |

## goals

- maintain a total BOM under $350 — source from aliExpress or local equivalents
- clean routing, logical layout, no overengineering
- no sensorless homing, no BLTouch, no CANbus
- firmware simplicity: just klipper + macros + serial UART for drivers
- design-first approach: firmware and wiring mapped before physical assembly
- optional: add side/rear panels to stiffen the frame or reduce airflow

## current state

- CAD: complete
- Firmware: configured and tested virtually
- BOM: under $340 with aliExpress pricing
- Printing: not started (deliberately deferred)
- Panels: optional files included, not essential

## documents

- [`journal.md`](./journal.md) – full design + firmware journal
- [`/firmware`](/firmware) – klipper firmware config
- [`bom.md`](./bom.md) – full component list w/ sources
- [`wiring.png`](img/wiring-diagram.png) – flat, labeled schematic using mermaidjs

took a lot less time than infill but had so much fun (other than having to use freecad. that stuff sucks i hate it srry i <3 foss but it is giving me ptsd in the future tense tbh aaah)

