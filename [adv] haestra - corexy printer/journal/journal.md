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
