# Cold-Chip README Patch (English, honest-positioning + hook)

Insert this block right after the title/badges, before "核心主张" (or replace the English-facing intro if you keep the doc bilingual). It's designed to do two jobs at once: land the hook in the first 5 seconds, then immediately show the reader you're not hiding the fences.

---

## The Claim, Stated Precisely

**A 1mm² chip architecture where a full 5-stage EDA verification pipeline reports 9,180 nW total power draw — a ~49,000x reduction over a conventional 3nm baseline in the same simulation environment.**

That number is real, in the sense that it's what Icarus Verilog 12.0 printed when this RTL was simulated. It is not a measurement of a physical device. Nobody has built this. Read on before you decide whether that matters to you.

## What "Zero Dissipation" Actually Means Here

The headline framing — "Q = I²Rt → 0" — describes the *architectural intent*, not a violation of thermodynamics. What the simulation models is a system where switching is done via optical microring resonance and a liquid-carried logic layer, instead of resistive electron transport. If the underlying physical assumptions hold in real hardware, resistive (I²Rt) loss is architecturally routed around rather than minimized — that's the bet. The 1.96% residual in the 98.04% EFF figure is the model's own admission that some dissipation floor remains.

**This is a claim about what the architecture would do if the physics behaves as modeled — not a claim that the physics has been verified.**

## Simulation Scope & Limitations

This is stated plainly, not buried:

- **All five verification steps (RTL / timing / power / DRC-LVS / stress) are behavioral simulations**, not measurements from fabricated silicon or a physical prototype. There is no lab hardware for this project.
- **The liquid-channel and microring parameters (refractive index, particle friction coefficient, switching thresholds) are modeled constants**, not values pulled from a characterized physical material system. Where they're not independently sourced from published material science, treat them as design targets, not verified physics — the same standard applied to the Photonic Logic Chip project.
- **The 3nm baseline comparison is a same-simulation-environment comparison**, not a fabricated-chip-to-fabricated-chip benchmark. Treat the 49,000x figure as an architectural delta under the model's assumptions, not an industry-benchmarked result.
- **DRC/LVS "14/14 PASS" refers to rule-check simulation on the layout model**, not a foundry design-kit sign-off.

## Why Publish an Unverified Architecture at All

Two reasons, both defensible to a technical reviewer:

1. **Priority timestamping.** The RTL, the architecture, and the specific mechanism (liquid-carried logic + microring switching to route around I²Rt loss) are disclosed here with a public, hashed record — establishing prior art regardless of whether this exact team fabricates it first.
2. **The simulation result is a specific, falsifiable target.** A 98.04% EFF figure under stated assumptions is something a photonics or liquid-crystal materials researcher can actually push back on with real numbers — which is more useful to the field than a vague pitch.

If you're a hardware architecture reviewer, materials scientist, or licensing evaluator: the fastest way to break this claim is to challenge the modeled refractive index and switching-threshold constants against published liquid-crystal / microring literature. That's an invitation, not a defense.

---

**Bottom line for licensing conversations:** this is a simulation-validated architecture with an unverified physical layer — the same category as an early-stage patent disclosure backed by modeling, not a working sample. Evaluate it as IP + a falsifiable technical bet, not as a de-risked product.
