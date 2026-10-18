# Spawner Finder — Fabric 1.21.11

Client-side ESP mod that highlights monster spawners through walls.
**Skeleton spawners by default (classic monster spawners only — no trial spawners)**, other spawner types optional.
Works in singleplayer and multiplayer (it only sees chunks the server has loaded for you).

## Download / file
- `spawnerfinder-1.25.0+1.21.11.jar` — the mod (put this in your `mods` folder)
- Full source code: `spawner-finder/` project in the workspace

## Requirements
1. Minecraft **1.21.11**
2. **Fabric Loader** (0.18.0 or newer) — https://fabricmc.net/use/installer
3. **Fabric API** for 1.21.11 — https://modrinth.com/mod/fabric-api
4. Java 21 (bundled with the Minecraft launcher)

## Install
1. Install Fabric for 1.21.11.
2. Drop **Fabric API** and **`spawnerfinder-1.25.0+1.21.11.jar`** into `.minecraft/mods`.
3. Launch the game with the Fabric profile.

## Controls
**Press `B` in-game to open the Spawner Finder UI** — start/stop the ESP there, pick mob types with the checkboxes, toggle the filled box, and set the range slider. Press `B` or `Esc` again to close it. Works in Survival, Creative and Spectator.

| Key / command | Action |
|---|---|
| **B** | Open / close the Spawner Finder UI |
| **F8** | Toggle spawner ESP on/off |
| **F9** | Cycle filter: Skeleton only → All spawners → Custom |
| `/spawnerfinder` | Show status (how many spawners are in your loaded chunks) |
| `/spawnerfinder filter skeleton` | Skeleton spawners only (default) |
| `/spawnerfinder filter all` | Every spawner type |
| `/spawnerfinder filter custom zombie` | Toggle a mob in your custom list (repeat for more mobs: `spider`, `blaze`, `cave_spider`, ...) |
| `/spawnerfinder filled` | Toggle the translucent filled box (outline only if off) |
| `/spawnerfinder range 128` | Set max range in blocks (16–512) |
| `/spawnerfinder ping` | Toggle the player-near-spawner ping on/off |

## Player ping alert
Tick **"Ping Me: Player Near Spawner"** in the `B` menu (on by default). When another player (not you, not a spectator) is within **16 blocks** of a spawner that matches your UI filter — e.g. someone AFK at a skeleton farm — you get:
- a **ping sound**,
- a **big on-screen alert** with the player's name and the spawner coordinates,
- a **chat message** with the same info.

It pings **once** when a player arrives at a spawner, then stays quiet until they leave and come back, so it won't spam you.

Keybinds can be changed in Options → Controls → **Spawner Finder**.

## Colors
- ⬜ **White** — Skeleton (and stray/bogged)
- 🟩 Green — Zombie · 🟥 Red — Spider · 🟦 Teal — Cave spider
- 🟧 Orange — Blaze / magma cube · ⬜ Gray — Silverfish
- 🟪 Magenta — anything else / unknown

## Good to know
- It highlights spawners in **loaded chunks only** — explore / raise render distance to find more. Nothing is scanned on the server side, so it's safe to use on servers, but the server must actually send you the chunk.
- Settings save automatically to `config/spawnerfinder.json`.
- If a spawner shows **magenta / "unknown"**, the server hasn't told your client which mob it spawns yet — getting closer usually fixes it.

## Building from source
Needs JDK 21:
```
cd spawner-finder
./gradlew build
```
The built jar lands in `build/libs/`.

## Area Miner (new)
Press `B` → **⛏ Area Miner**. Select the area two ways:
1. **Look & keys:** look at a corner block, press `[` for Pos1 and `]` for Pos2.
2. **In the menu:** type Pos1/Pos2 coordinates and click *Apply Coords*, or use
   the **custom amounts**: type any numbers for **Width**, **Length**, **Down**
   and **Up** (Down defaults to **50** — yes, a full 50 blocks down) and click
   *Set Area*. It centers on what you're looking at, or on you.
   **Order** picks the digging direction: Top → Down, Bottom → Up, or Nearest (sideways).
   **Extend** picks which side the area grows from where you stand/look, relative to
   the way you face: Centered, **To My Left**, **To My Right**, Forward, or Behind Me.
   You can also **name your selection** in the Name field (e.g. "base hole") — the name
   shows in the status line and in the start/finish messages. *Auto-Name* fills one in for you.

The selected box shows in yellow in the world. Press **▶ Start Mining** (menu
closes, mining begins): it mines top-layer-first, block by block like a player —
instant in Creative, tool speed in Survival. Creative flight flies to
out-of-reach blocks; Survival walks, sprints, steps off blocks it's standing on and tries to unstick itself; if it's stuck 3 times it pauses and tells you. Still no pathfinding — watch for falls/holes. **Bedrock and fluids are never mined**,
and with *Safety Stop* on (default) it auto-stops before breaking a block that
touches water/lava. Max area 50,000 blocks. Pause/Stop from the same menu.
Use it only in your own world or on servers where you have permission.

**Movement fix (v1.7):** if the miner can't reach the block it wants, it now
breaks what's in the way first — the block under its feet to dig down to it,
or blocks in front at feet/head height to push through — then continues to
the real target. It only ever breaks blocks inside your selected area.

## Click GUI (v1.9)
Press `B` and the menu is now a click-GUI like classic clients: dark panels
(**FINDER / MINER / ALERTS / MISC**) with module rows and round toggle dots.
Click toggles to flip them, left/right-click value rows (Filter, Range,
Order, Movement, Mine Speed, Extend, Ping Range) to change them, and drag
panels around by their headers. Same features as before — nothing else added.

## RTP Hunt (v1.10)
In the `B` menu, **HUNT** panel: press **▶ Start RTP Hunt** and it sends
`/rtp overworld` for you again and again (delay 10/30/60s — respect your
server's /rtp cooldown) until another player (not you) is within ping
range of a **skeleton spawner only**. Then, your choice:
**Alert: Spam Me** (sound + big title + chat spam every second) or
**Alert: Kick Me** (disconnects you with the find in the message).
**Stop When I Move** (default on): moving by hand stops the hunt, and
stops the spam after a find. Needs a server that actually has /rtp.

**v1.10.5:** the hunt now also finds **BASES** (5+ chests/beds/furnaces/hoppers/shulkers close together near your landing). On ANY find it **kicks you** and the disconnect screen says **SKELETON FOUND** (player + coords) or **BASE FOUND** (coords). Spam mode removed.

**v1.10.6:** HUNT panel now has three RTP buttons: **Skeleton RTP** (person at skeleton spawner), **Player Base RTP** (player base blocks only), and **Both**. All kick on find with SKELETON FOUND / BASE FOUND reason.

## Attack Escape (v1.10.7)
B menu -> **ESCAPE** panel (on by default). The moment a player or mob damages you: a Totem of Undying is moved from your inventory to your **offhand** and **`/rtp overworld`** is sent to escape. Fall/fire/lava damage does not trigger it. Toggle **Also Flee From Mobs** off to only flee from players. 15s cooldown so it can't spam. (Needs totems in your inventory and a server with /rtp.)

**v1.10.8:** auto-totem also equips at low health — at **5 hearts or less** a totem from your hotbar or backpack is moved to your offhand automatically (any damage source), so it's already in hand before a killing blow lands.

## AH Flip (v1.11.0)
B menu -> **AH FLIP** -> Start Auto AH Flip. It opens `/ah` on the server you're connected to (no IP needed — it reads that server's live auction house), scans every page reading prices from item lore, compares listings of the same item, buys the cheapest one when it can be re-listed higher, then lists it with `/ah sell <price>` just under the next price. Profit = sell - buy; it shows the flip it picked in chat and loops (max 10 flips, Stop anytime in the panel). Needs: money to buy, free inventory space, an /ah with prices in lore and click-to-buy (confirm button supported). First run: watch it — if your server's /ah layout differs (buttons/sell syntax), report what happened.

## Netherite Finder (v1.12.0)
B menu -> FINDER -> **Netherite Debris ESP** (on by default): ancient debris in loaded chunks gets a **magenta** through-wall box. Pure client-side chunk reading, sends nothing. Anti-xray note: servers with ore obfuscation send hidden debris disguised as netherrack, so fully buried debris is NOT in your chunk data and cannot be highlighted by any client mod; debris the server reveals (exposed/uncovered) is found.

## AH Flip v2 (v1.13.0)
- **Server IP settings**: B -> AH FLIP -> **Server IP / AH Settings**. Type your server IP, auction command (Auto = try /ah then /auction, or /ah only, or /auction only), AH **tax %**, and Auto /orders on/off. (Calculation always uses the live AH of the server you're connected to — that's the real market data.)
- **Real profit**: sell price is the next seller undercut by 1%, and a flip is only taken if price **after your tax %** is still above buy price. Set your server's real tax in settings or it can sell at a loss.
- **Endless**: it no longer stops after one item — buy -> sell -> rescan -> next flip, forever, until you press Stop. No profitable flip? It waits 30s and rescans instead of quitting.
- **/orders auto**: after each AH scan it checks /orders — fulfils profitable requests from your inventory, or buys the cheapest AH listing to deliver into a better-paying order. Order count: orders fulfilled.

**v1.13.1 fix:** the price reader no longer trusts `~ $...` lore lines (that's the server's average per-item price, not the listing cost — it bought 9 diorite believing ~$137 that really cost $1K). It now uses the explicit total Price line (per-item prices are multiplied by stack size), and only buys when profit after tax is at least 3% over buy price.

**v1.13.2:** FINDER panel -> **Test Base Kick (30s)**: enable it and 30 seconds later you're kicked with a **BASE FOUND** disconnect screen (your current coords), so you can verify the kick works. It switches itself OFF after firing — when you rejoin it's disabled.

## Chunk Base Finder (v1.14.0)
B -> FINDER -> **Chunk Base Finder** (on by default). Scans loaded chunks for base blocks (4+ chests/beds/furnaces/barrels/hoppers/shulkers/enchanting/beacons in a chunk, including underground) and marks the chunk with a **green glow box + beam** up to the surface. "Base chunks: N" shows how many found in range. Loaded chunks only.

**v1.14.1 crash-guard:** every feature tick (miner, hunt, escape, AH flip, finders), chunk scan and finder render is now wrapped — a bug in one feature logs an error instead of crashing the whole game on join.

**v1.14.2:** Chunk Base Finder is player-bases only — villages and trial chambers (beds/chests/furnaces alone) no longer glow. A chunk needs player-placed blocks: a shulker box / ender chest / beacon / conduit (+4 base blocks), or 2 strong blocks (enchanting, lodestone, respawn anchor, anvil) + 6 base blocks. The actual base blocks are now also outlined in green, not just the chunk.

**v1.14.3:** Chunk Base Finder detection rebuilt — bases are judged as clusters across chunk borders (3x3 chunks), so a normal underground base (5+ base blocks, 3+ kinds, 3+ storage) now glows even without rare blocks. Villages/trial chambers are suppressed by their markers (bell, trial spawner, vault) instead.

## v1.15.0
- **Exclusions**: Chunk Base Finder no longer counts/highlights respawn anchors or ender chests (obsidian was never highlighted) — too common in generated structures.
- **Watcher HUD**: top-right corner shows "WATCHING YOU" in red with names/distances of players whose crosshair is pointed at you (64 blocks). Toggle in B -> ALERTS.
- **UI overhaul**: colored panel headers, shadows, zebra rows, toggle pills, hover accents, and a bottom status bar (spawners / debris / bases / hunt / AH / escape status).

**v1.16.0:** B menu rebuilt as a big client window (Krypton-style): title bar (draggable), left category sidebar (FINDER/MINER/ALERTS/HUNT/ESCAPE/AH FLIP/MISC with module counts), large scrollable module cards, scrollbar, and a live status footer. Window up to 780x470.

## v1.17.0
- **Spectate**: B -> SPECTATE lists nearby players; click one to see through their eyes (your body stays where it is — stop from the same panel). Reopen the panel to refresh the list.
- **UI like the screenshot**: title bar says "Hello, <you>", live clock in the middle, a working **Search modules...** box (click it and type — filters every module; Backspace deletes, Enter/Esc unfocus), and module cards are now **two columns** with Enabled/Disabled subtext and a red active dot.

**v1.18.0:** UI reverted to the original click-GUI (draggable colored panels, 2-row grid, toggle pills, status footer) — the big Krypton-style window/search/two-column cards are gone. Only Spawner Finder's own modules are in the menu (none of the combat modules from any screenshot). Watcher HUD aiming check fixed to use head rotation.

**v1.18.1:** Watch Player moved into the FINDER panel (with Chunk Base Finder): click **Watch Player (Type Name)**, type a player's name, and Watch puts you in their POV (loaded players only; your body stays put). Stop Watching is right below it. The separate SPECTATE category is removed.

**v1.18.2:** Watch Player is any-range: if the typed player isn't loaded near you, it tries spectator + teleport to them (`/gamemode spectator`, `/teleport <name>`, `/spectate <name>`) and locks onto their POV when they load. Needs server permission (OP on your/friends' server). Stop Watching returns you to your original position and gamemode. If the server denies the commands, it reports that after ~12s.

**v1.18.3:** any-range Watch retries harder: staged /tp, /teleport, /gamemode spectator + /tp, /spectate attempts over 30s, checks the tab list to confirm the player is online, and if the server denied permission it tells you to run `/op <yourname>` in the server console.

**v1.18.4:** Watch Player / spectate feature removed completely (screen, camera logic, FINDER rows). Watcher HUD (players watching YOU) is a different feature and stays.

## Mine Until Base (v1.19.0)
B -> MINER -> **Mine Until Base (auto-extend)**: select an area like normal, start it, and when the area is mined out it automatically extends the same size area in the direction you were facing and keeps mining. The whole time, Chunk Base Finder watches loaded chunks; when a base appears (ignoring anything within 24 blocks of where you started, e.g. your own base), the miner stops with a big **BASE FOUND!** title + coords + ping. Stops itself after 30 empty (all-air) areas in a row.

**v1.19.1:** MINER -> **Fast Mining (Turbo)** toggle: zero pause between blocks, near-instant head turns and fast fly/walk while mining. In Survival the pickaxe's real break time still applies; in Creative blocks break instantly.

**v1.19.3:** Mine-Until-Base builds its own tunnel now (3 wide x **Tunnel Height you choose** x 16 long, extending in your facing direction) — set **Tunnel Height: N up** (2-12) in the MINER panel before starting. No manual area selection needed.

**v1.19.4:** Mine-Until-Base no longer stops outside when a base is found: it steers a tunnel straight to the base center (through rock or open cave) and only stops once you're inside (within ~3 blocks of the base center), with a **BASE FOUND — YOU'RE IN!** title.

**v1.19.5:** Mine-to-base steering fixed: it now tunnels **at your own Y level** toward the base in segments (no giant box mined up to the surface), and only digs a small 3x3 shaft up/down when horizontally next to the base.

**v1.19.6:** Mine-Until-Base never stops on its own: removed the 30-empty-areas stop (it walks through caves instead), stuck recovery no longer pauses it, and the water/lava safety pause doesn't apply in this mode. It only stops when a base is found (and you're inside) or when you stop it.

## Auto Builder (v1.20.0)
B -> **BUILDER** panel. Modes (walk around, it builds):
- **BRIDGE**: hold any block, walk — it places blocks under your feet (bridge across anything, even sprinting).
- **PLATFORM**: same but lays a 3x3 floor under you as you move.
- **PRINT**: select a finished build with the miner selection ([ ]), click **Copy Template**, stand at the bottom-corner of the new spot, turn Builder on in PRINT mode, and walk around — it rebuilds the template block by block, auto-grabbing the right blocks from your hotbar.

**v1.20.1:** Area Miner size limit removed — select and mine an area of any size (huge areas take a moment to count/scan at start).

**v1.20.2:** Area Miner limit set to **500,000 blocks** per selection.

## Spawner Finder upgrade (v1.21.0)
- **Pin Nearest Spawner** (B -> FINDER): pins the closest spawner in white in the ESP; the Pinned value row left-click cycles to the next spawner, right-click clears.
- **Spawner HUD** (top-left, toggleable): shows pinned or nearest spawner with mob, distance, compass direction and coords at all times.

**v1.21.1:** UI upgrade (same drag-panel layout): bigger panels and rows, gradient headers with category icons, larger toggle pills, brighter value/action markers.

**v1.21.2:** AH Flipper upgrade: after each scan it queues ALL profitable flips (best first) and works through them — a sold-out/broke flip moves to the next instead of restarting. After /ah sell it verifies the stack actually left your inventory (really listed); if not, it retries with /auction sell and reports instead of pretending.

**v1.21.3 SAFE FLIP:** AH Flipper can't go minus by itself anymore — needs 5+ listings of an item, prices from the median of other sellers (troll-proof), cheapest must be 10%+ under market, hard break-even floor, received-count verification with re-pricing. New AH Settings: 'Min Profit (SAFE)' (default +15% after tax). Set your server's real tax % there too (default 5%).

**v1.21.4:** AH Flipper works on ANY server IP with /ah (or /auction) — no IP setup needed; it uses the server you're connected to. Server field is now an optional label only.

**v1.21.5:** UI fix — removed emoji header icons (Minecraft's font can't render them, they showed as garble), slimmer rows so panels no longer run under the bottom status bar, solid footer bar.

**v1.21.6:** click-GUI panels are scrollable (mouse wheel inside a panel, max 12 rows shown, header shows position like 12/14) so ALL panels/rows are reachable at any window size or GUI scale.

**v1.21.7:** menu shows ALL rows in every panel again; mouse wheel anywhere scrolls the whole menu page up/down (no per-panel scrolling).

**v1.21.8 FAST FLIP:** AH Flipper ~10x faster — pages scanned every 3 ticks (was 10), rescan pause 2s (was 30s), buy/confirm/sell steps near-instant; fixed it declaring listings 'gone' before the page loaded (2s grace); relaxed second pass (3+ listings) still guarded by the no-loss floor so it actually finds flips on quiet markets.

**v1.21.9 AUTO SELL:** new AH FLIP row 'Auto Sell My Inventory' — scans /ah for market prices, then lists your inventory stacks one by one at 98% of market median with /ah sell (verified listed; unsellable stacks are skipped, items without AH price data untouched). The flipper's own sell step is the same verified flow.

**v1.21.10:** Auto Sell prices for profit — lists so you keep 105% of market median AFTER tax (no undercutting). Flipper remains floor-guarded (min profit after tax).

**v1.22.0 MOB AIM ASSIST (PvE):** B -> FINDER -> 'Mob Aim Assist (hold attack)'. While you hold attack, aim smoothly pulls onto the nearest animal/monster within range (Aim Range 8-32, 60-degree cone, smooth capped turn speed). Never targets players.

**v1.22.1:** Aim Assist now targets ALL mob types (bats, slimes, fish, etc.), not only Animal/Monster classes.

**v1.22.2:** Aim Assist now activates while holding RIGHT-click (aim/lock), leaving left-click free to punch/attack normally.

**v1.23.0 AUTO KILL BATS:** B -> FINDER -> 'Auto Kill Bats'. Fully automatic: fast predictive aim tracks the nearest bat within 24 blocks (leads its flight path) and auto-punches the moment it is within reach, respecting weapon cooldown. Status row shows tracking/punch state.

**v1.23.1:** Auto Kill Bats now CHASES the bat — runs after it on foot (sprints, jumps when it's above/blocked) or flies straight at it in Creative flight, until you press stop (toggle off). Stops moving when in punch reach or no bat is near.

**v1.23.2:** Auto Kill Bats chase is walk-only (no sprint, no fly, no jump).

**v1.23.3:** Auto Kill Bats is smarter when stuck: if it barely moved for ~1s while chasing, it ducks (sneaks) for 1.5s to squeeze under/through the blockage, then stands and continues (status shows 'stuck — ducking under…').

**v1.24.0 EXPLORER PACK:** (1) Auto Elytra Explorer (HUNT panel): auto-flies your elytra at cruise height on your heading, auto-fires hotbar fireworks, and stops with a BASE FOUND title+coords when Chunk Base Finder sees a NEW base. (2) Elytra ESP (FINDER): pink boxes on elytras in End City ship item frames and dropped elytra items (unopened chest contents are invisible to any client). (3) Amethyst ESP (FINDER): lavender boxes on amethyst clusters/buds in loaded chunks — finds geodes.

**v1.24.1 Elytra Explorer fixes:** smart rockets (only fires when flight speed actually drops; selected slot restored after firing); hotbar refilled from inventory rockets automatically; auto-totem in offhand while exploring; stops with an ELYTRA ALMOST BROKEN warning at 25 durability left.

**v1.25.0:** (1) Elytra Explorer sends the real START_FALL_FLYING packet itself (the 'click to fly') — no manual click needed. (2) Amethyst Finder: chunks with 6+ amethyst now flag BASE NEAR — big lavender box + beam highlight; counter shows base-near spots. (3) Freecam (FINDER panel): camera flies freely (WASD, jump up, sneak down, sprint fast) while your body stays put; toggle off to return.
