# Mario Kart Arcade GP 2 - Skip Checks Patch

Patches for Mario Kart Arcade GP 2 (USA - GGPE02) made by Claude AI to bypass arcade hardware checks and boot directly into gameplay on Dolphin Triforce.

## What This Patch Does

**ISO Patch (MKAGP2_SkipChecks.xdelta):**
- Skips the 60-second satellite cabinet link wait (timer reduced from 3600 frames to 1)
- Bypasses NamCam2 camera check
- Bypasses steering wheel initialization/error check ("Wheel error", "e20", "Please call an attendant")
- Skips game test mode hardware validation
- Fixes seat loop and stuck initialization loops

**Dolphin Config (GGPE02.ini):**
- 99 credits (runtime patch)
- Infinite race time Gecko code

## Requirements

- Mario Kart Arcade GP 2 (USA) ISO (Game ID: GGPE02, Rev.00)
- Dolphin Triforce branch (tested on build 2512-397)
- `xdelta3` to apply the patch

## How to Apply

### 1. Apply the ISO patch

```bash
xdelta3 -d -s "Mario Kart Arcade GP 2 (USA).iso" MKAGP2_SkipChecks.xdelta "Mario Kart Arcade GP 2 (USA) [patched].iso"
```

Or on Windows with xdelta UI: select the original ISO as source, the .xdelta as patch, and choose an output filename.

### 2. Install the Dolphin config

Copy `GGPE02.ini` to your Dolphin Triforce game settings folder:

- **Portable Dolphin:** `<dolphin folder>/user/GameSettings/GGPE02.ini`
- **Linux installed:** `~/.local/share/dolphin-emu/GameSettings/GGPE02.ini`
- **Windows installed:** `%USERPROFILE%/Documents/Dolphin Emulator/GameSettings/GGPE02.ini`
- **Android:** `Android/data/org.dolphinemu.dolphinemu/files/User/GameSettings/GGPE02.ini`

### 3. Dolphin controller setup

Set **SP1** and **Port 1** to **AM-Baseboard** in Dolphin's controller settings.

Default controls:
| Action | Button |
|--------|--------|
| Gas | R Trigger |
| Brake | L Trigger |
| Use Item | A |
| Insert Coin | Z |
| Test Menu | Guide/Home |

### 4. Launch the game

The game may not appear in Dolphin's game list (Triforce titles often don't). Use **File > Open** to launch it directly.

## Technical Details

14 instructions patched in the main DOL executable (56 bytes total):

| Address | Original | Patched | Description |
|---------|----------|---------|-------------|
| `0x8003737C` | `li r0, 3600` | `li r0, 1` | VS wait timer init |
| `0x800374BC` | `li r0, 3600` | `li r0, 1` | VS wait timer init |
| `0x80037710` | `li r31, 3600` | `li r31, 1` | VS wait timer init |
| `0x80037870` | `li r30, 3600` | `li r30, 1` | VS wait timer init |
| `0x80037FBC` | `li r31, 3600` | `li r31, 1` | VS wait timer init |
| `0x80084850` | `li r0, 3600` | `li r0, 1` | Global satellite timer |
| `0x80073BD8` | `stb r6, 0x25(r5)` | `stb r3, 0x25(r5)` | Disable camera check |
| `0x800BE10C` | `mr r4, r3` | `b +0x2C` | Seat loop bypass |
| `0x8002E100` | `beq -0x158` | `nop` | Stuck init loop fix |
| `0x8028B5D4` | `blt +0x38C` | `nop` | 60-second loop bypass |
| `0x8002E340` | `bne +0x20` | `nop` | Game test mode bypass |
| `0x8002E34C` | `stb r0, 0(r25)` | `nop` | Game test mode bypass |
| `0x80084FC4` | `beq +0xC` | `b +0xC` | Seat loop bypass |
| `0x80085000` | `bne +0x10` | `nop` | Seat loop bypass |

## Notes

- Do NOT apply DVDInquiry or CMD Encryption patches if your Dolphin build is 2512-395 or newer. Those are handled natively and applying them will cause "Unhandled Media Board Command" errors.
- The 99 credits and infinite time patches are runtime-only (in the .ini file) and do not modify the ISO.
- Timer patches reduce the 60-second arcade cabinet sync wait to effectively instant.
