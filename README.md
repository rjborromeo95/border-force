# Border Force — lean shift only

A browser playtest rig for the lean shift of Border Force. Plain HTML, CSS and
JavaScript: no build step, deploys to Vercel as a static site. The other
modes (standard and policy shifts) and their art are gone from this repo;
some of their code is still in `game.js` and `items.js` but nothing can reach
it.

See **RULES.md** for how to play.

## Uploading to GitHub from the browser

GitHub's web uploader takes **at most 100 files per upload**. This repo has
about 240 files, so it comes in four parts, each under 100:

1. **part 1** — code, signs, sounds, icons (the shell)
2. **part 2** — cards: books to fish
3. **part 3** — cards: hats to t-shirts
4. **part 4** — cards: totes to underwear, and the suitcases

Unzip each part and drag its **contents** (the `assets` folder and any loose
files) onto the repo's "Add file → Upload files" page, one part at a time,
committing each. Order does not matter. Vercel redeploys after every commit,
and the deploy after the last part is complete.

If a picture is missing on the live site, a red note in the bottom-left corner
names the file. The offline cache no longer keeps failed loads, so once the
file is uploaded it appears on the next visit.

## Where things live

- `assets/items.js`:
  - `LEAN_PIECES` — every piece, its two sides and their tags. A piece is
    forbidden if any tag on either side matches a red sign.
  - `LEAN_CATEGORIES` — the signs.
  - `LEAN_COLS` — how wide the grid is.
  - `leanDeck()` — deals the pieces. Random side up, tools never left out.
- `assets/game.js`:
  - `GAMES.lean` — bag count and size.
  - `setUpBoard()` — shuffles the wall.
  - `TOOLS` / `offerTool()` — screwdriver, lighter, fish, bomb and knife.
    - The gun is not a held tool: `seizeGun()` sets `gunLock`, which skips
      the whole tool step before the next bag, for both players.
    - The fish flips a whole row, `rowReach()`; rows are lettered A–F.
    - The bomb reshuffles the wall and re-deals the same number of reds,
      `detonate()`.
  - `oppTools()` / `oppStabs()` — how Officer B uses them.
  - `creditSeizure()` — scoring. `useTool()` ends the game when the wall
  goes bare.
  - `tumble()` — the random angle a piece comes out at.
- `sw.js` — the offline cache. Bump `CACHE` and every `?v=` in `index.html`
  together whenever the code changes.

## Art

Cards are 440×617 transparent PNGs. Both sides of a piece share one scale, so
turning it over never changes its size. Signs are 240×240 transparent PNGs.

- **Balls** now uses the new balls pair; the bowling-ball pair is no longer
  used.
- **Books** uses the books pair, and **Underwear** the underwear pair.
- **Towels** and **Fish** use the pairs supplied with those pieces.
- **Teddies** uses the updated no-teddies sign. It is saved as
  `no_teddies.png`, a new name, so no phone keeps the old one cached.
- **Devices** uses the `devices_allowed` / `no_devices` pair already in the
  art.
- **Cameras** (`camera_allowed` / `no_camera`), **Shoes** (`shoes_allowed` /
  `no_shoes`) and **Bombs** (`bombs_ok` / `no_bombs`) also come from the art
  folder.
- **Guns** use `guns_ok` / `no_guns`.
- **Snacks** use `snacks_allowed` / `no_snacks`.
- **The two AK-47s** touched on their sheet and were split apart along the
  narrowest point between them.
- **The dynamite** was drawn once, so its back is its mirror image. Replace
  `lean_bombs_1_b.png` if a proper back gets drawn.
- **Bottles** uses the alcohol pair; its icon is a bottle, but it could have
  its own art.

## Known design questions

- **Pace.** Every sign that starts red is a tool (knives, lighters, screwdrivers, fish, bombs), so scoring starts slowly
  until the first tools are seized and used.
- **The bare-wall ending.** A wall with no red ends the game, and the most
  seized wins. Officer B knows this: when ahead and able to clear the whole
  wall with one lighter, it does. Otherwise B leaves at least two reds up
  and never burns the Screwdrivers sign while screwdrivers are going round.
- **A possible stall.** If the only red signs left are ones whose pieces
  have all been seized, nothing more can be forbidden and no screwdriver can
  be taken legally. The game then runs until the bag-circulation limit ends
  it. It is rare, but the rule could extend to "no forbidden piece left in
  play".
