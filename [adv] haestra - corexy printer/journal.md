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

## day 15 (date 06/25) | hours 41 - 43 | 20:45 - 23:45
tonight was mostly about merging parts into subassemblies in fusion. i hadn’t realized how much clutter i’d left in the top-level model — dozens of sketches floating in space and two versions of the gantry rail.

moved all toolhead components into a single component. discovered the Y carriage offset was inconsistent depending on how i mated the MGN block. redid all mates from scratch. tried capturing design history but fusion kept slowing down — disabled it for now.

exported the cleaned-up assembly and checked rough weight balance. toolhead + carriage combo comes to about 185g. might need to reduce the blower duct thickness.

also tested a few ways to lock the belt clamp inside the carriage. tab + compression pad seems better than pinching — less deformation.

## day 16 (date 06/27) | hours 44 - 46 | 22:15 - 01:15
started creating wiring placeholders. made a sketch layer for rough cable routing: steppers, endstops, hotend, bed, and fans. nothing precise, just a planning pass.

placed JST-style connectors around the toolhead zone. want the harness to enter from behind the X rail, through a strain relief. still debating whether to route cable chain from the back of the printer or from the top.

did a quick Klipper board power analysis ,,, ooo 5015 + hotend + steppers will sit well under 12V 240W total even with buffer. noted some cheaper PSU options on aliExpress ($16 shipped for 12V 30A). still unsure if i want 12V or 24V fans though.

cut a hole in the back of the top extrusion for future drag chain mount. won’t finalize it until the toolhead cabling is clearer. confusion ensues. wisdom tooth pain.

## day 17 (date 06/29) | hours 47 - 49 | 19:00 - 22:00
firmware tonight. fleshed out `printer.cfg` w rough definitions for all steppers and default pinouts assuming Manta M4P board. X and Y steppers mapped to `PA0` and `PA1`, Z on `PB3`, and E0 left unconnected for now.

set microstepping to 16 with interpolation, acceleration to 3000, jerk limits low. no PID tuning yet. bed size defined as 220x220x240. safe Z-home active.
also added a virtual `[bed_screws]` section and placeholders for `input_shaper` macros. dropped in a commented `mcu temperature` section just in case.
klippy simulator showed no errors. saved the config in `/printer_config/haestra-printer.cfg`.

## day 18 (date 07/01) | hours 50 - 53 | 20:30 - 23:30
finally addressed the gantry rail clamping. was using a plain notch in the XY mounts, but that caused twist under belt tension in simulation.
switched to a dovetail rail cut with a press-fit printed block. added two vertical compression bolts. simulated belt pull at 1.8kg && block held without deformation.

also tried a mockup of sensorless homing with TMC2209. klipper lets me enable stealthchop by default, and fallback to spreadcycle for homing. might need extra current during homing, which could trigger false alarms on long moves. logged this as a TODO for later.

poked around in klipper macros again. drafted a placeholder macro for `G28` that sequences X/Y/Z in stages, and a basic `PAUSE` macro that moves to 150x150x10.

## day 19 (july 3) | hours 53–56 | 14:00 – 17:00
bit of a slow warmup. kept opening the z assembly and just poking around. i think the double pulley setup i made for the z sync belt was making the belt loop too long & ended up reworking the position of the anchor so it sits closer to the tension side. might still have too much slack.

i also started sketching a breakout pcb mount near the toolhead. thought i could sneak it into a small corner under the x rail, but there’s not enough clearance for a connector header and cable strain relief. maybe zip-tie to the drag chain bracket instead. not elegant, but i’m tired. lowk was hoping to also get the fan duct outline started but couldn’t focus by the end.

** btw my hours include research / scrapping / brainstorming also **

## day 20 (july 4) | hours 56–59 | 16:00 – 19:00
got the rear panel layout reworked with slightly tighter spacing between the psu and board. angled the board just a bit to make space for the drag chain mount. doesn’t look super neat in cad, but it solves like three problems at once so i’m sticking with it.

finally decided to move the z belt tensioners fully under the bed instead of keeping them at the back. it’s gonna be annoying to tension post-assembly but cleaner from a routing perspective. will need to mirror the idler holes and redo the anchor tabs.

also reduced the main rear extrusion from 2040 to 2020 just to test, saved about 140g total. might keep it. idk. my tooth hurts.

## day 21 (july 5) | hours 59–62 | 13:00 – 16:00
back to gantry. started messing with carriage endstop mounts again, this time testing a magnet + hall sensor idea. i was going to just stick with a microswitch but realized i could save wiring and enclosure space with a magnetic trigger, assuming the magnet doesn’t drift over time.

also went back and thickened the motor plate fillets. they felt too fragile, especially around the lower belt mount slots. probably overkill but i’m paranoid. tweaked the step file export again just to check for errors. somehow i’ve ended up with three different folders named “final-final-v6”.

## day 22 (july 6) | hours 62–65 | 11:00 – 14:00
decided to stop being precious and finalize the belt layout. moved the a/b motors out by another 5mm to get better clearance behind the idler flanges. sketched in some simple printed belt clamps that should tuck just inside the extrusion slots.

also re-angled the board again. added a printed tray under the baseplate for mounting it sideways. not elegant but this version might allow for cleaner airflow across the heatsink side. midway through i realized the drag chain anchor was floating in midair. forgot to model the base bracket. added it, cleaned up the routing, and re-exported the base frame step.

## day 23 (july 7) | hours 65–69 | 09:30 – 13:30
long session. filled out a lot of missing details. started the fan duct again, this time anchoring to the toolhead base instead of the carriage face. gave it a chamfered inlet and made sure it doesn’t hit the drag chain. still needs mount holes and cable path.

finally modeled the z motor mounts for real. just rectangular blocks with a recessed mount and enough clearance for the pulleys to drop in. nothing fancy.

took a break and came back to sketch the top frame cover,, haven’t decided if it’s acrylic or not, but i left 3mm spacing and added tabs just in case.

## day 24 (july 8) | hours 69–72 | 13:00 – 16:00
last push before firmware. cleaned up all model warnings and exported a “final-ish” step. realized i was missing some cable strain relief paths so i modeled in two little notches on either side of the rear extrusion. added screw bosses for printed zip tie loops.

also simplified the toolhead backplate so i could swap in different fan duct versions without taking apart the carriage. might revisit that later but it’s good enough for now. interview tmrw. checked overall fit again. still nervous about the belt tensioner access, but it’s staying.

## day 25 (july 9) | hours 72–78 | 22:00 – 04:00
started with a barebones config and slowly mapped things pin by pin. triple z setup took the most time. i assigned each motor to its own driver, then configured `[stepper_z1]` and `[stepper_z2]` to support later gantry leveling. it’s a 3-leadscrew lift, all independent, which should be nice once everything’s squared up.

had to sit down and sketch the full pinout again just to keep track. uart assignments were a mess to trace, i forgot how much guessing is involved when the datasheet doesn’t match the silkscreen.

stepper direction tuning was annoying !!! fr i had to mentally flip the entire axis diagram twice because of how the motors are mounted. i'm sure i'll still get x wrong first try sensorless homing was tempting but no im not gods strongest soldier so im sticking to switches for now. modeled mounts, added a `[endstop_phase]` section, set conservative speeds.
thermistors: e3d for hotend, bed using generic 100k. hardcoded pullups seem fine.

then… macros. got into it.
- `G32` mapped to gantry leveling  
- `G34` for z tilt  
- added `print_start` + `print_end` w manual purge lines (even though i don’t have a nozzle)  
- ended up just enabling resonance test in config with dummy values for now

split the config into `printer.cfg`, `extruder.cfg`, and `mcu.cfg`, probably unnecessary, but easier to keep sorted when it all starts breaking (which it will). spent forever on a dumb typo in `[heater_bed]`. was missing a comma. whole config refused to load. im a genius.
mmmh stared at screen and 3 empty cans of redbul next to me as the sun rose (rise? rised? ykwim) no flashing yet, but ready to. paid my respects to the 3dp gods and crashed. 

might be it unless there's smth else. idk. too much going on im overwhelmed i miss fusion my baby and lowkey feel like a bad mother (???) for neglecting it idk how my mind works but yeah fusion ur my #2 fav only child. tinkercad *fire-emoji*

<img src="img/film.gif" width="100%" height="100%" />