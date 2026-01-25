# Studio MIDI Architecture & Permanent Wiring Plan

> **Goal:** A finished, calm, modular studio where MIDI is boring, reliable, and never needs re‑thinking.

This document defines the **permanent wiring layout** and **rules** for all MIDI‑related devices in the studio. It is intended to be stored in GitHub as the single source of truth.

---

## Core Design Principles

1. **USB is for humans** (controllers you touch).
2. **DIN MIDI is for machines** (hardware synths).
3. **The audio interface is the border** between the two worlds.
4. **One device = one job.**
5. **No cleverness, no overlap, no memory required.**

If a setup violates any of the above, it is wrong.

---

## System Overview (Mental Model)

```
Humans (Controllers) ──USB──► Computer (AFRICA)
                                   │
                                   └──USB──► Audio Interface (MOTU UltraLite‑mk5)
                                                     │
                                                     └──DIN MIDI──► Hardware Synths
```

Keep this model in mind at all times.

---

## Layer 1 — Desk Controllers (USB ONLY)

These devices live at the workstation desk and **connect directly to AFRICA via USB**.

### Devices
- Mini‑key MIDI keyboard (notes only)
- 4×4 pad controller (drums only)

### Wiring
```
Mini‑key controller  ──USB──► AFRICA
Pad controller       ──USB──► AFRICA
```

### Rules
- Controllers **never** connect to the MOTU via DIN MIDI.
- Controllers **never** talk directly to hardware synths.
- Controllers are **input only**.
- One USB cable per controller, permanently installed.

### Roles
- **Mini‑key controller**
  - Purpose: Melody, harmony, note entry
  - MIDI Channel: 1
  - Data sent: Note on/off, sustain pedal, pitch/mod (optional)

- **Pad controller**
  - Purpose: Drums only
  - MIDI Channel: 10
  - Data sent: Note on/off only

### Absolute Bans
- No knobs mapped to mixer levels
- No transport controls
- No “All MIDI Inputs” in DAWs

---

## Layer 2 — Audio Interface as the MIDI Gateway

The **MOTU UltraLite‑mk5** is the *only* bridge between:
- USB ↔ DIN MIDI
- DAW ↔ hardware synths
- MIDI clock ↔ machines

### Wiring
```
AFRICA ──USB──► MOTU UltraLite‑mk5
```

### Rules
- MOTU is **always** connected directly to AFRICA (never via USB hub).
- MOTU provides the **only MIDI clock** in the system.
- No controllers plug into the MOTU’s MIDI IN.

---

## Layer 3 — Hardware Synth Wall (DIN MIDI ONLY)

Hardware synths live on shelving ~15 ft from the desk. They are **never connected to AFRICA via USB**.

### Devices
- Novation Supernova
- Korg MS‑20 mini
- Arturia MiniBrute

### MIDI Wiring (Current: THRU Chain)

```
MOTU MIDI OUT
   │
   ├──► Supernova MIDI IN
   │        │
   │        └──MIDI THRU──► MS‑20 MIDI IN
   │                           │
   │                           └──MIDI THRU──► MiniBrute MIDI IN
```

### Channel Assignments (example – can be adjusted)
- Supernova → MIDI Channel 2
- MS‑20 mini → MIDI Channel 3
- MiniBrute → MIDI Channel 4

### Rules
- Each synth listens on **one fixed MIDI channel**.
- MIDI THRU only — never chain from MIDI OUT.
- No synth sends MIDI back to the computer unless explicitly required (default: OFF).

### Future Expansion
If more hardware synths are added:
- Add a **MIDI Thru box** near the synth shelving.
- Do **not** change desk wiring.

---

## USB Port Usage (Expected)

| Device | USB Ports |
|------|----------|
| Mini‑key controller | 1 |
| Pad controller | 1 |
| MOTU UltraLite‑mk5 | 1 |
| Mouse / keyboard | 1–2 |

If additional ports are needed:
- Use a **powered USB hub** at the desk for controllers only.
- Never place the MOTU on a hub.

---

## Failure Modes This Design Avoids

- MIDI feedback loops
- Multiple clock sources
- Intermittent DIN MIDI from controllers
- Long USB runs to synth shelves
- Re‑mapping controllers every session
- “I’ll remember what this knob does” syndrome

---

## One‑Sentence Contract

> **USB handles people.**  
> **DIN MIDI handles machines.**  
> **The MOTU is the border crossing.**

If a future change violates this contract, stop and redesign before proceeding.

---

## Status

- This wiring plan is **authoritative**.
- Templates, DAW settings, and future hardware must conform to this document.
- Changes require updating this file first.

