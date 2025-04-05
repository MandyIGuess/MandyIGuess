# NSMBW Hooks and Code Patches
A collection of code patches and changes that'll work with NewerSMBW and NSMBWer+ (unless specified otherwise).<br>
For bigger features, check out my [NewerSMBW-Modding][modding] repo!

All addresses are from the PALv1 release of the game. No credit is needed if you use any of these.

To install any hooks into your own mod, add them to `bugfixes.yaml` or similar.

## Layouts/User-Interfaces
This patch will make the stars on your savefiles always draw the sparkle effects, even if you've gotten a Super Guide block.
```yaml
  - name: AlwaysDrawSavefileStarEffects
    type: nop_insn
    area_pal: 0x8077DD2C
```
These patches will remove the Free Mode (Free-for-All) and Coin Battle buttons from the File Selection screen. They cannot be selected, and the animations to show/hide them are disabled.
```yaml
  - name: FS_RemoveMultiSelect
    type: patch
    addr_pal: 0x80783D04
    data: '4800001C'

  - {name: FS_DisableMultiIn,  type: nop_insn, area_pal: [0x807837FC, 0x8078380C]}
  - {name: FS_DisableMultiIn2, type: nop_insn, area_pal: [0x8078394C, 0x8078395C]}
  - {name: FS_DisableMultiOut, type: nop_insn, area_pal: [0x80784220, 0x80784230]}
```
This patch will make number printing functions use the default width numbers rather than the fullwidth numbers.<br>This only noticably affects MessageFont, such as in the W9 Star Coin Board messages.
```yaml
  - name: UseNormalWidthNumbers
    type: nop_insn
    area_pal: 0x800CDEBC
```

## Miscellaneous
This will stop the Super Guide bit (0x40) in the savefile bitfield not be toggled when dying 8 times in a stage.
```yaml
  - name: DontToggleSuperGuideBit
    type: nop_insn
    area_pal: 0x800CE364
```
This prevents players from losing lives when dying.
```yaml
  - name: DontLoseLives
    type: nop_insn
    area_pal: 0x8006066C
```
This patch allows you to change the duration of the Star powerup.<br>
Replace `0294` with the number of *frames* you want it to last for (the game runs at 60fps).
```yaml
- name: ChangeStarPowerupDuration
  type: patch
  addr_pal: 0x80A28430
  data: '38A00294' # 11 seconds
```
This patch will remove the cutscenes and credits after the final boss. Once beaten, you will simply return to the World Map.
```yaml
  - name: WorldMapAfterFinalBoss
    type: patch
    addr_pal: 0x807CF404
    data: '38600003 38800000' # scene = 3 (WORLD_MAP), settings = 0
```

[modding]: https://github.com/MandyIGuess/NewerSMBW-Modding
