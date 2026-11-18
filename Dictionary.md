# FlightPlanner Dictionary

The terms used in the FlightPlanner app (FlightPlannerApp) and its planner (FlightPlannerCore), with what they mean here. Part 1 is what you see on screen; Part 2 is the planner's own terms, as used in the code, design notes and run outputs.

**Units.** On screen: heights in feet, speeds in knots, distances in nautical miles, times in minutes (m:ss on the node card). Inside the code everything is metric (metres, m/s, km) and converted for display. "MSL" is height above sea level; "AGL" is height above the ground directly below.

---

## Part 1 — In the app

### Task, weather and glider

**Task plan** — The MSFS flight plan file (.PLN) of a task: the start, the turnpoints and the finish. Loaded with File ▸ Load Task Plan. Each waypoint's id carries its rules, for example `*Start 7000+3517|7000x5000`:
- the name (`Start 7000`; a leading `*` marks the start or finish gate),
- `+3517`: the waypoint's ground elevation, ft,
- `|7000`: the highest you may pass it, ft MSL (the start's maximum height),
- `/…`: the lowest you may pass it, ft MSL (when given),
- `x5000`: the cylinder diameter in metres, or for a start or finish the line length in metres,
- `;AAT;`: an assigned area rather than a turnpoint.

**Task setup** — The dialog that opens straight after Load Task Plan, showing what Plan Route will use: the weather preset (with Change…), the glider, the resolution and the MacCready. OK applies them; Cancel keeps the filled-in weather and leaves the glider, resolution and MacCready as they were. To change them later, load the task plan again.

**Weather preset (.WPR)** — The MSFS weather file for the day, normally in the task's folder. The planner reads the day from its name, for example `SSC 155 Low Cu T8-12 W15 - 24v2`:
- `W15`: the surface wind, kt (direction from the file). In MSFS the wind is full strength up to about 820 ft above ground and tapers above, and the planner uses it that way.
- `T8-12`: a thermal day, with its thermal strength range. Such a plan's summary carries a thermal-day caution.

**What-if glider** — The glider you place on the map yourself (Shift + click), when not connected to the sim. Dropped with a click within 0.5 nm of a plan line, it starts at the plan's height there, so the node card reads as on plan; Shift + drag from open map samples the height where you press and drops the glider where you let go, 1,000 ft above the sampled height (usually higher above the ground than a plain drop). Shift + drag on the glider rotates it. **Remove** in its popup takes it off the map (no arrival heights, no next node) until it is dropped again. Click it to edit its height (MSL), glider type, wind and MacCready. Its glider type is the one Plan Route plans for; Task setup sets it too.

**Sim aircraft** — Your aircraft in MSFS when the app is connected to the sim: position, height, airspeed and wind come from the sim once a second.

**Wind vane** — The wind arrow on the map. Task setup and Plan Route set it to the weather preset's wind.

### Lines on the map

**Route line** — A line you draw on the map (Ctrl + clicks; release Ctrl to finish). Its ends and its highest point are labelled with ground elevations. The Add / Delete drop-down switches between drawing and deleting (Ctrl + click near a line).

**Guide line** — A route line drawn while planning, which Plan Route treats as your preferred line for one leg: it flies to it, along it in the direction you drew it, then on to the turnpoint. It is kept within 300 m (about 0.16 nm) either side of what you drew. A guide line belongs to the leg it adds least distance to. It is ignored, and the plan summary says why, if it is more than 10 km (5.4 nm) from that leg, if two legs fit it equally, or if what is left outside the task cylinders is too short. Parts inside a cylinder are dropped.

**Plan line** — A line drawn by Plan Route: one per leg, along the planned route, with its nodes. It is coloured by what the glider does along it (a legend shows while a plan is on the map):
- **green, ridge**: model lift at the plan's height of 2 kt (1 m/s) or more, for at least 0.3 nm;
- **red, glide**: the rest of the line;
- **orange, sink**: modelled lee sink of 1 kt (0.5 m/s) or more, for at least 0.3 nm;
- **purple dot, climb**: where the plan circles for thermal height, larger for more height (full size at 1,000 ft).
Routes saved before this (14 Oct 2026) draw as plain red lines.

**Estimated arrival height (AGL labels)** — The height above ground you would have on arriving at a point of a line, flying straight there from the what-if glider or sim aircraft. Shown at line points and used for the node card's first comparison.

**IGC track** — A logged flight loaded with File ▸ Load IGC. Click it for the pilot, height above ground and height above sea level at the nearest fix. A task flight is drawn only as far as its task counted: to the finish, to a landout, or to the fix before a slew. The popup adds "(landed out)" or "(slewed)" when a track was cut short.

### Plan Route

**Plan Route** — File ▸ Route ▸ Plan Route: plans the loaded task with the weather and glider from Task setup, and draws the plan lines. The status history and the log record the weather, glider and resolution used.

**Leg** — The stretch from one waypoint to the next (Start → WP 1, …, WP 4 → Finish).

**Tag point** — Where a leg meets the next turnpoint's cylinder: the point the plan turns at.

**Cylinder** — The circle round a turnpoint you must enter (diameter from the plan's `x` value).

**Start line / finish line** — The gates at each end of the task, square to the first or last leg, of the length in the plan's `x` value.

**Node** — A point on a plan line where you do something or must have something. Kinds:
- **START / TP**: the start and each turnpoint's tag point.
- **CLIMB**: where the plan takes thermal height.
- **RIDGE**: joining a working face (model lift of at least 1 m/s, about 2 kt, for at least 3 km, about 1.6 nm).
- **LEAVE RIDGE** (Ridge end): leaving it.
- **CREST**: crossing a ridge line less than about 1,300 ft above it; the crest stands at least about 500 ft above the ground either side along the line.
- **PASS**: a crest that is also a dip across the line (a col); the ground rises either side.
- **SINK**: a stretch of modelled lee sink of 0.5 m/s (about 1 kt) or more for at least 3 km.
- **LOW POINT**: the leg's lowest height above ground (excluding its last 2 km).
- **ROUTE**: a filler where nothing else happens for 10 km (5.4 nm).
- Nodes within 2 km (1.1 nm) of each other are merged; the card shows every kind merged ("CREST + RIDGE").

**Next node** — The node the what-if glider or sim aircraft is heading for. It has a yellow ring and its card is shown. A node counts as reached within about 0.5 nm. More than about 1.6 nm off the plan, the next node is the nearest one ahead within 60° of your heading.

**Node card** — The panel for a node. Its lines (for plans made from 14 Oct 2026; older routes keep an earlier layout):
1. **Name @ plan height · distance · plan time.** The plan time is the plan's own time to reach the node from your position. A passed node shows "passed".
2. **The plan's instructions**, two lines:
   - **To here**: the stretch into the node (from your position when it is the next node), e.g. "Ridge: climb 760 ft at 51 kt, then thermal 170 ft" or "Glide: lose 420 ft at 102 kt";
   - **Then**: the stretch from the node to the one after it (none after the finish), so you see what comes next.

   The stage is what covers most of the stretch (or "ridge then glide" when mixed), the height is the plan's own change and the speed is the plan's airspeed. A thermal is named where the plan takes it: at a Climb node, the end of the stretch into it ("…, then thermal 170 ft"); on a turnpoint, after rounding it, the start of the stretch leaving it ("Thermal 80 ft, then glide …").
3. **"You: ±ft vs plan here"**: your height against the plan's height where you are. Its colour is the card's verdict:
   - **green**: at or above the plan,
   - **amber**: below the plan but more than 330 ft above the ground,
   - **red**: less than 330 ft above the ground,
   - **grey**: no position yet.
4. **The straight-glide check** (flying straight to the node on the polar, no lift):
   - on a glide stretch, "Straight glide: arrive ±ft vs plan";
   - on a ridge or climb stretch, in grey, "No lift: clears ±ft" — what happens if the lift isn't there.

Drag the card to move it; double-click to put it back.

**Inspecting a node** — Press the mouse on a plan node to see its card (blue ring, blue card outline). Drag onto another node to switch, and release to go back to the next node. With the profile open, its blue line moves to the node, on the node's leg, while the mouse is held.

**Profile** — File ▸ Route ▸ Show Profile: a draggable panel with the plan's height along one leg at a time (◀ ▶ to step). The ground is filled, a dashed line marks the plan's floor 330 ft above it, the plan's height is coloured as on the map (ridge, glide, sink), climbs are purple dots and the turnpoints are at each end. Hover for distance, plan height, ground height and clearance; a blue dot shows the same point on the map. Drag the title bar to move it and the grip at the bottom-right corner to resize it; place and size are kept, and double-clicking the title bar puts both back. Routes saved before 14 Oct 2026 have no profile: plan again to see it.

**Resolution** — How finely each leg is planned: factor × 24 control points per 60 km of leg (File ▸ Route ▸ Resolution, or Task setup). ×1 puts a point every 1.35 nm, ×2 every 0.67 nm, ×3 every 0.45 nm. A finer line can follow a ridge instead of cutting across it, but takes longer to plan. The factor is kept between sessions (settings.json, `PlanResolution`) and shown in the plan summary header.

**CPU Use** — How many processors Plan Route may use: Light (half), Normal (all but one), Full (all). While planning, the app runs at below-normal priority so Windows stays responsive. Kept between sessions.

**Compare with Free Plan** — With guide lines drawn, also plan the task without them and report the difference per leg and in total. About doubles the run time; work already done in the session is reused.

**Plan summary** — The page shown after each re-plan (and with File ▸ Route ▸ Show Route Summary), also saved beside a saved route as `<name>_summary.md`. Its columns and terms:
- **Header:** weather preset, glider, MacCready, resolution, and when it was planned.
- **Total:** the plan's time (h:mm and minutes), its length (nm) and the thermal height climbed (ft).
- **Flight min:** time flying the line.
- **Thermal:** the height the plan climbs in thermals on the leg, and the time it takes.
- **Total min:** flight plus thermal time.
- **Depart ft / Arrive ft:** heights MSL leaving and arriving at the leg's turnpoints.
- **Lowest no-lift ft AGL:** the lowest height above ground on the leg where the model has no ridge lift to recover on.
- **Guide:** how many guide lines the leg follows, and their length.
- **vs free:** with Compare with Free Plan, the leg against the plan without guide lines.
- **Flags:**
  - not flyable (the line does not clear the terrain),
  - committed (8 nm or more flown under about 1,640 ft above ground with lift under 0.5 m/s, about 1 kt),
  - low (the lowest no-lift height is under about 490 ft above ground).
- **Thermal-day caution:** on a thermal day (a `T` token in the preset name) the plan climbs in thermals only by circling. Pilots also take thermals in straight flight where the plan cannot know them, so its ridge choices need reviewing.

**Saved route** — File ▸ Route ▸ Save Route writes the plan lines, any guide lines and the summary to a .json file; Load Route puts them back.

**Status history** — The list of actions under the status line: what was loaded, planned and with what.

**Clear All** — Starts the app afresh, as if just opened: forgets the task, weather, plans, lines and the session's planning work.

### Ridges (Ridge Analysis menu)

**Scan Terrain for Ridges** — Finds ridge lines along the task from the terrain alone. It starts from points standing above both their neighbours along at least one direction, and grows them along the crest.

**Re-Analyse Ridges** — Scores the ridges found for the current wind: length, slope and alignment with the wind first, then the windward face from terrain cross-sections.

**Ranked Ridges** — The scored ridges, rated and coloured by their energy yield. Save, load or clear them from the menu; files are named `<task>-<wind>-<glider>-rankedRidges.json`, the wind as compass direction + knots (`SW15`, `NE5`) from the weather preset's name, else from the session wind, in a folder you choose — the task's folder at first — and the discovered ridges are saved beside them as `…-discoveredRidges.json`.

**Energy yield** — How much height a ridge gives per distance flown along it: ft gained per nm at the glider's best-glide speed, in the predicted ridge lift, over the ridge's best continuous stretch (at least 0.5 nm, or the whole ridge if shorter, not past a blocked spur). Per unit length: a ridge's length is seen on the map, and a long ridge is often flown only along its good part. It uses the glider's own polar, so the same ridge yields more to a slower glider. Ratings: Poor below 0 (height lost even in the lift), Weak 0–130, Usable 130–270, Good 270–400, Strong 400 ft/nm and over. The popup shows it, e.g. "Yield: +396 ft/nm at 70 kt over 0.5 nm".

**Ridge presets** — Settings for ridge discovery: High Altitude (Alpine), Lowlands, and Debug.

**Manual Ridge Identification** — Mark ridges on the map yourself.

### Keys

**T / N** — Track up / north up. **C** — Centre the sim aircraft. **R** — Clear the netto trail (the circles the aircraft leaves behind it, showing netto: the air's own rise or sink). **D** — Delete the route lines.

---

## Part 2 — In the planner

**Elastic band** — The route search for one leg. A line of control points between the two waypoints is moved sideways, a little at a time, keeping every move that makes the leg faster, until no move helps. It samples the lift model and terrain wherever the points go, not on a fixed grid.

**Control points** — The movable points of the band. Their number per leg is set by the resolution (6 to 240 a leg). Between them the band is straight, so their spacing limits how closely a line can follow a curving ridge.

**Seed** — One starting shape for the band: a gentle bulge either side of the direct line. Several seeds are relaxed and the fastest flyable result is kept. Results within 0.5 min of each other are tied, and the one with the most ridge support wins.

**Optimal heights** — How the band's line is flown: for each stretch between control points, the best airspeed and the height to be at (on a 25 m grid, speeds in 4 kt steps), across the whole leg at once. Pilots hold speed and let height come and go; this flies the line that way.

**Lift model** — The ridge lift the planner expects at a point and height. It comes from the terrain's slope facing the wind, including ridges upwind whose lift carries downwind and up. It scales with the wind at the glider's height above ground, and is capped about 980 ft (300 m) above the windward crest. Slopes facing away from the wind give lee sink once the downward forcing passes a threshold. Checked against about 410,000 logged fixes.

**Netto** — The vertical movement of the air itself, without the glider's own sink. What the lift model predicts and what IGC files log.

**MacCready** — The expected climb rate in thermals. Plan Route's is set in Task setup (3.9 kt, 2.0 m/s, by default: the value the planner was tuned and scored at) and does two things:
- **the price of thermal height**: the plan assumes climbs at 0.75 × MacCready, so 1,000 ft costs 6.6 min at 2 kt, 3.4 min at 3.9 kt and 2.2 min at 6 kt;
- **the cruise speed** between climbs: stronger expected climbs justify flying faster and losing more height.

So a **higher** MacCready makes thermal height **cheaper**: the plan cuts corners and circles more. A **lower** one makes it dearer: the plan keeps to the ridges and flies slower. It changes which line wins, not only the times, and plans at another value are not comparable with the corpus scores. The what-if editor has its own MacCready (0 kt, best glide, by default) for the arrival estimates; it does not affect Plan Route.

**Thermal height (height debt)** — Height the plan needs that ridge lift does not give it — below the minimum height above ground (100 m, about 330 ft, en route), or short of the arrival height. It is bought by circling, priced at 0.75 × MacCready. Shown as "Thermal" in the plan summary.

**Arrival height** — At least 100 m (330 ft) above the ground at each tag point and the finish, or the turnpoint's minimum height when the plan gives one.

**Turnpoint heights** — The height to arrive at each turnpoint, chosen across the whole task. Each leg is timed from every departure height to every arrival height (100 m steps, up to 2,000 m above the turnpoint's floor), on four lines. The fastest sequence of heights is picked, then each leg is planned again at those heights.

**Flyable** — The line clears the terrain all the way at the plan's heights.

**Committed** — A stretch flown low (under 500 m, about 1,640 ft, above ground) with no ridge lift to recover on (under 0.5 m/s). Reported only: a fast but committed line is shown as such, not replaced.

**Plan cache (session cache)** — Results kept for the app session: each leg's solution and height-time table, under a key of everything that decides them. Planning again with guide lines, another comparison or the same settings reuses what has not changed. Cleared by Clear All and by loading a different task.

**Concurrency limit** — The cap CPU Use sets on how much planning work runs at once.

**Development corpus / held-out tasks** — The tasks used to tune and check the planner against real pilots (Innsbruck, Kastelorizo, Monneilleur, Nikolsdorf, St Johann, Crépin, Klippeneck, Torino and others). The held-out tasks (California, Combolcha, Herrere, Montana, North Connel) are never used for tuning or debugging; they test the result.

**Plan / fastest** — A plan's time divided by the fastest pilot's time on the same task: the main score of a corpus run. Under 1 means the plan is faster than the best pilot.

**Briefing check** — A test that a plan matches what a task's briefing says (for example "conserve height on the headwind leg"). It checks the plan only; it never feeds the planner.

### IGC files

**IGC file** — A logged flight: a header, then records by letter:
- **B record**: one fix (time, position, heights, and the logger's extra fields such as airspeed, netto and GND).
- **C record**: the task the pilot declared, one waypoint per line, with the same id format as the task plan.
- **L record**: logger notes, including SLEW ON / SLEW OFF.

**Fix** — One logged position (a B record), usually once a second.

**GND** — The logger's ground-contact field on each fix: the glider is on the ground.

**Slew** — Moving the aircraft in the sim by hand (SLEW ON in an L record). Anything after a slew is not flown.

**Proper start** — The start that counted: the last start-line crossing before the pilot reaches WP 1. Earlier crossings, landings and slews before it don't count against the flight.

**Landout** — The first ground contact after the proper start. The flight ends there; anything after it (a relaunch, a recovery) is not part of the task.

**Task end** — Where a task flight's counted part ends: the finish, a landout, or the fix before a slew. IGC tracks are drawn to here, and the analyses stop here.
