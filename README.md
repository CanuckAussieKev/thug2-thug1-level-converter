# THUG2 THUG1 Level Converter

> **Not affiliated with or endorsed by Activision, Neversoft or any other rights holder.** A non-commercial fan project
> for preservation, interoperability and education. No game files are included: the tools work only on copies of the
> games you own. Game names are trademarks of their owners. See LICENSE (MIT for the project's own work; no licence for material
> from the games); third-party parts (downloaded tools) are listed in THIRD_PARTY_NOTICES.md. Rights holders: open an
> issue and it will be dealt with promptly.

Converts THUG1's main, classic and skate shop levels, with their scripts, people and cars from your own copy of Tony Hawk's Underground (PC) to the formats of Tony Hawk's Underground 2 (PC), for its level
select. Nothing of either game is in this repository: the files are made from your copies each time. How it works,
what is converted and what isn't: [thug1_levels/README.md](thug1_levels/README.md).

- `thug1_levels/` - the converter (`thug1_levels.ts`: `convert`, `selfcheck`, `list`), its readers of THUG1's files, its people and the THUGPRO comparisons.
- `thug1_campaign/` - the THUG1 campaign survey: what THUG1's story scripts call that THUG2 lacks (`script_gap.ts`, `thug1_sizes.ts`).

## Dependencies

Each pinned to a tagged commit in package.json (`git+https://git@github.com/...#<commit>`; one copy of each: every repository that uses one pins the same commit):

- [thug2-level-converter-core](https://github.com/CanuckAussieKev/thug2-level-converter-core): THUG2's formats (scenes, collision, textures, archives, scripts), the level port's common steps and the THUGPRO compatibility register. THUG1's own formats are read here; THUGPRO's hand edits to its THUG1 levels are applied with core's `compat/hand_edits.ts` (the content pipeline applies the same edit sections to THUG2's own levels).

## Public API

What other repositories may import - package.json's `exports`, nothing else (`import ... from "thug2-thug1-level-converter/thug1_levels/driving.ts"`):

- `thug1_levels/driving.ts` - the THUG2 forms of THUG1's driving scripts (PC-THUG1-22): the content pipeline re-applies them to Anteara Mod's copy of car_scripts.qb.

Commands (package.json's `commands`: the tools with a command line - importing a module never runs one): `thug1_campaign/script_gap.ts`, `thug1_campaign/thug1_sizes.ts`, `thug1_levels/migrate_layouts.ts`, `thug1_levels/thug1_levels.ts`, `thug1_levels/thugpro_compare.ts`.

## Usage

Needs Node 24 and NeverScript's `ns.exe` (QB scripts compiled): the
[THUG2 decompilation](https://github.com/CanuckAussieKev/thug2-decomp) builds it (`tools\build_neverscript.ps1` ->
`out\neverscript\ns.exe`, found there when this converter is used inside it) - or pass `--ns <ns.exe>`.

    git clone https://github.com/CanuckAussieKev/thug2-thug1-level-converter
    cd thug2-thug1-level-converter
    npm ci
    node thug1_levels/thug1_levels.ts convert NJ --thug1 "C:\Games\THUG1" --thug2 C:\Games\THUG2 --ns <ns.exe>
    node thug1_levels/thug1_levels.ts selfcheck --thug2 C:\Games\THUG2

`convert` writes `<thug2>\thug2-decomp-thug1\` (not read by the game); without level names it converts the default
set (thug1_levels/README.md). The [THUG2 content pipeline](https://github.com/CanuckAussieKev/thug2-content-pipeline)'s
`menu_level/make_menu_level.ts` then installs every converted game's levels into the game's data overlay and level
select. In the decomp, where the pipeline installs this converter, the same commands are

    node tools/pipeline/run.ts thug1_levels/thug1_levels.ts convert --thug2 C:\Games\THUG2
    node tools/pipeline/menu_level/make_menu_level.ts --game C:\Games\THUG2

Checks: `npm run typecheck`, `npm run lint`, `npm test` (the tests that need a game install skip without it;
`package.test.ts` checks package.json's exports, commands and pins). GitHub Actions runs them on every push
(`.github/workflows/ci.yml`).

## Versions

Releases are tagged `v<major>.<minor>.<patch>`; this is `v1.0.0`. The repositories that use it pin the tagged commit.
