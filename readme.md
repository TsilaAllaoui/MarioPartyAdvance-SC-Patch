# Mario Party Advance - WAITCNT Patch for SuperCard Compatibility

This patch disables the game's attempt to modify the WAITCNT register during mini-game transitions, preventing black screen crashes on SuperCard and other flashcarts with slow ROM access timing.

---

## ✨ Summary

- **Game**: Mario Party Advance (USA)
- **Issue**: During mini-game transitions, the game writes to `WAITCNT` (0x04000204), which can cause issues on flashcarts like SuperCard.
- **Cause**: The game performs a `SWI 0x11` (CpuSet) to copy data from ROM to RAM. In that copied data is the value `04 AB 14`, which is then dynamically manipulated to construct the address `0x04000204`.
- **Fix**: Patch the ROM using the provided IPS patch to modify the `AB` byte, preventing the game from forming the correct address and safely avoiding the WAITCNT write.

---

## 🔧 Technical Details

- The game tries to write to WAITCNT after starting a mini-game and stores only the partial address of WAITCNT in IWRAM (the first byte `04`).
- It reconstructs the full WAITCNT address (`0x04000204`) at runtime via logic on values in IWRAM.
- The IPS patch changes `AB` to `00` in the relevant ROM block, so the final constructed address is invalid or read-only, making the attempted write a no-op.

---

## ✅ Status

- Confirmed working in multiple mini-games
- No WAITCNT modification occurs after patching

---

## 📥 How to Apply

1. Download the provided `.ips` file.
2. Use an IPS patcher (e.g. Lunar IPS or MultiPatch) to apply the patch to a **clean USA ROM** of Mario Party Advance.
3. Save the patched ROM and load it on your SuperCard or emulator.

---

## 🧪 Notes for Testers

- Not all mini-games may use the same routine. Please report any mini-game that still triggers WAITCNT writes.
- Other replacement values (besides `00`) may also prevent the issue while improving compatibility.

---

## 🚀 Goal

Create a clean and widely compatible patch for Mario Party Advance to prevent WAITCNT writes that can crash flashcart hardware.

Contributions welcome!
