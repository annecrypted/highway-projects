# haestra journal - advanced highway project log

## day 01 (date 06/02) | hours 1 - 1 | 02:00 - 03:00
new project: haestra. the idea came back while i was flipping through some old printer builds. i want something like a corexy, but tighter, denser, quieter, sharp lines. think: <200mm³, no spaghetti belts, no ~~~~zazzy~~~~ gantries.
no cad, mostly just sketched and set goals:
- pure 2020 frame  
- clean corexy belt layout (non-stacked preferred)  
- 2 z rods, 1 motor  
- mgn12 linear rails all axis  
- toolhead must stay slim  
- minimal printed parts  
also: keep the total BOM under **$350**, so be smart about sourcing parts. 
basically i was trying to visualize the motor placement without eating up Z height.

## day 02 (date 06/03) | hours 2 - 4 | 22:00 - 01:00

reopened the base frame sketch and realized i didn’t account for t-slot clearance in the corner brackets. started resizing every corner gap to make space for actual fasteners, not just perfect unions.

also spent almost 40 minutes staring at the XY motion layout. tried 3 belt paths before realizing they were all basically the same, just mirrored. i still can’t decide between the stacked vs non-stacked belts. bcs likeeeee non-stacked feels cleaner, less tall but it’s gonna need a printed crossover support near the rear left motor, and i don’t love that.

i think i’m gonna try a rotated coreXY layout, where the belts form a triangle instead of a rectangle. needs careful tensioning, but i like the vibe. started placing pulleys and sketching their bolt hole distances.

## day 03 (date 06/04) | hours 5 - 7 | 20:00 - 23:00
spent the whole evening modeling the stepper bracket. got halfway before realizing the motor shaft sticks too far out & it clips the gantry rail by ~3mm.
backed up and redesigned the motor mount as a two-part bracket: a printed angled faceplate and a bolt-on tensioner arm. i modeled the arm as a 2020 inset to save material and clamp directly onto the frame. it flexes in CAD, though. might need ribs.

also did some napkin math on belt length. GT2 belt perimeters look like they’ll land around ~940mm. i think a single 5m closed loop will cover XY and z-sync if i route it smart.

## day 04 (date 06/06) | hours 8 - 10 | 22:30 - 01:30

x-gantry time. used a 2020 crossbar with MGN12H carriage underneath, running along Y. hotend carriage rides directly on it. spacing between gantry and belt paths is now only 7.2mm, just enough for a zip-tied belt to not rub.

couldn’t decide on how to clamp belts to the carriage. toyed with tensioners, pins, even weird sliding T-nuts, but ended up with a notched block + bolt-down plate. oldschool.
also changed rail mounts to use M3x6mm with t-nuts, but i don’t have the low-profile heads modeled yet. made a note to check if i can find them cheap in ali bulk packs.

stalled out a bit trying to decide if the XY tensioners should float or be fixed into the frame. leaning float, it'll mean less frame deformation under belt load.

## day 05 (date 06/08) | hours 11 - 13 | 22:00 - 01:00
no modeling today, just firmware config and pinout stuff.
mocked up a klipper config for the MCU: probably gonna use a skr mini e3 v2 since it’s what i have on hand. tried mapping XY to TMC2209s and z to a single 2208. haven’t defined heaters yet.

configured basic corexy kinematics and added:
- `rotation_distance: 40.0` for XY (16T pulley, GT2)
- `microsteps: 16`
- `endstop_pin` config for virtual XY min
- z stepper at `rotation_distance: 8.0` (TR8x2 guess)

flashed an old .bin just to test power-on. got heartbeat. no errors. it lives!

## day 06 (date 06/10) | hours 14 - 16 | 23:00 - 02:00
z-axis modeling. i keep redoing this because it’s so dumbly hard to route a clean Z motion chain inside a compact frame. settled on two smooth rods (8mm) and one TR8 leadscrew in the middle, rear-side. carriage blocks will be printed and ride dual linear bearings. leadscrew is constrained by a nut in the bed platform and a motor below with a flexible coupler.

spacing is tight. bed is gonna be like 160x160 max. thought about buying MGN12 rails for z too but they’re like $16 each even on ali. sticking with rods.
made a placeholder bed plate in CAD just to check bounds. it’ll probably sit on three screws,,,, triangular layout for tramming.

## day 07 (date 06/12) | hours 17 - 19 | 20:30 - 23:30

went back to the XY motor mounts. realized my original idea had the belts entering the pulleys at a shallow angle — risk of rubbing on the pulley rim.

tried reshaping the bracket but couldn’t get enough clearance without raising the whole gantry by 4mm, which i don't want. settled on rotating the steppers slightly outward. reparameterized the bracket so pulley centers sit 11.5mm from edge of 2020, then aligned gantry rail centerline.

cad kept bugging out on me. one constraint loop refused to solve until i rebuilt it from scratch. burned 40 minutes on one sketch dimension, because i forgot fusion doesn’t like projecting sketches between contexts.

also found a cheaper 16T idler listing on ali with proper flanges + 5mm bore. $6.80 for 10 — cheaper than local resellers. saved the link.

## day 08 (date 06/14) | hours 20 - 22 | 21:00 - 00:00
modeled the XY idler blocks and tried to make them clamp directly into the 2020 extrusion slots without any T-nuts. doesn’t work. extrusion corners round off just enough that the printed block rocks side to side under tension.

added small alignment tabs on the underside, just enough to seat the part. now using one bolt into a slot nut to secure. sketched a new profile that locks the GT2 belt path with a small ledge. modeled both front and rear blocks, mirrored.

also added mounting holes for possible tensioner screws later. don’t want to overcomplicate the first pass.

measured total belt path length — about 970mm per loop. might need to route Z belts under the bed instead of through gantry if i want everything tight.

## day 09 (date 06/15) | hours 23 - 25 | 22:00 - 01:00
bed platform again. tried placing the smooth rods at 140mm spacing but my carriages overlap with Z nut at the rear. shrunk it to 120mm and offset the TR8 screw a bit forward, now the bed centerlines properly and i get better screw access too.

modeled a rough bed carriage: basically a flat plate with three bolt holes for leveling screws and a rear block that slides over the nut. the block was too wide so i cut out 4mm off each side and made it asymmetrical.

also drew up a little part to hold the Z smooth rods at the top, just a slotted printed cap that bolts into the extrusion with one M5. works okay in CAD but could probably flex.

firmware side: added `safe_z_home: true` and set `position_min: -2` for Z to give me some headroom during probing later. configured virtual endstops for corexy motion but still haven’t decided how i’ll wire physical switches.

## day 10 (date 06/16) | hours 26 - 28 | 20:00 - 23:00
i started working on a printable toolhead mount that fits a dragon clone hotend — but then realized it’s too tall and puts the nozzle 24mm below gantry. huge waste of Z travel. scrapped it.

modeled a new carriage based around a short e3d v6 clone block (21mm tall, aliExpress $8.25). designed it to slide into the MGN12H carriage with two M3s and a keyed print notch. added slots for zip-tied belts. forgot belt exit path though.

belt clamps were off by 2.5mm ,, i’d drawn them based on belt centerline, but didn’t account for tooth clearance. redrew the clamping plate with two pins and a recessed screw hole. belts now tuck cleanly without curling. considered switching to 9mm belts, but they’re ~1.6x price and overkill for this frame. sticking to 6mm GT2.

## day 11 (date 06/18) | hours 29 - 31 | 21:00 - 00:00
decided to fix the XY belt routing completely. i’d left the return path vague but turns out the belt touches the top MGN rail if i keep the steppers too low.

redesigned the motor mounts again. this time i lowered the motor face by 3mm, cut out a pocket so the belt returns under the rail without collision. now the idler block sits just above the lower extrusion and clears the pulley.

also tried slotting the belt tensioner screw through the motor mount but it looked messy. moved it into the idler block instead — side-mounted M3 with a compression tab.

ran out of time while doing sketches for the rear corner blocks. they’ll need belt routing, extrusion joining, and maybe LED slotting if i ever add lighting.

## day 12 (date 06/20) | hours 32 - 34 | 22:30 - 01:30
spent an hour just hunting down mgn12 cad files that actually match what aliExpress clones ship. most of the official Hiwin ones have extra chamfers that aren’t realistic.

remodeled the carriage to use real-world bolt spacing: 20mm square, with slightly oversized slots to compensate for cheap tolerances.

used parametric offset in the belt anchor sketch so i can bump the belt grip 0.25mm if needed. kept holes symmetrical, no zip tie path for now.

also started sketching an optional cable chain anchor at the back of the carriage. very basic loop slot with 2x m3 holes. undecided if it’ll be side-mount or top-exit.

klipper config: added `[input_shaper]`, `[mcu]`, and the `[stepper_*]` blocks just as placeholders. no pin mapping yet. thinking of using a spare Manta M4P, but might switch to SKR Mini for price. both support sensorless homing.

## day 13 (date 06/21) | hours 35 - 37 | 19:30 - 22:30
more z axis fixing. i think i overconstrained the bed in CAD oooo so the smooth rods sit perfectly parallel, but the nut block now jams when the rods are spaced less than 124mm. probably tolerance stack-up from too many fixed mates.

tweaked the carriage sketch to use construction geometry instead of direct constraints. added ±0.2mm side clearance on the nut mount. also tried modeling a fixed pulley for future bed belt lift (just in case). not pursuing that right now,, might make the whole thing wobble under load.

switched to manual screw lift for now. will lock Z stepper into place under the bed using a printed plate and two side clamps bolted into the lower 2020 extrusion. still unsure how to brace the bottom of the TR8.

## day 14 (date 06/23) | hours 38 - 40 | 21:15 - 00:15
spent most of tonight rethinking the hotend mount again. i tried to reuse the V6 clamp idea from before but this time i split it into two vertical halves with a dovetail key.

needed it to be tool-free but still rigid. the solution was two lateral screws that pull the halves together, clamping the groove mount in place. added a built-in 5015 blower mount facing sideways, with slots for zip ties.

realized mid-way that the part would interfere with the X belt path if mounted directly on top of the MGN block. so i raised the whole hotend by 2mm and shifted belts slightly outward.

created a basic duct placeholder,,, just a half-shell with an M2.5 insert planned. too small for real testing yet but gives me volume constraints for later.
