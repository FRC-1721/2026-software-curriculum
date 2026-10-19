# Week 2 — Mentor Facilitation Guide
### Java for FRC, part 1: values, methods, objects

**One-line goal for the hour:** every student writes a small Java helper class (`RobotMath`), proves it works with a passing test, and calls it from robot code on a live joystick axis.

**You are teaching:** a mixed group. Weeks 2–3 are the Java on-ramp, so the split is at its widest — new students learn what a method *is*; experienced students write tests and think about design. Keep pairs experienced↔new.

**Format:** standard 60-minute template (10 warm-up · 10 concept+demo · 35 lab · 5 wrap).

**Pairs with:** the Week 2 student lab and the `java-basics-week2` starter pack (verified: `RobotMath` + JUnit tests compile clean and pass).

---

## Before class — mentor prep checklist

- [ ] Everyone's Week 1 project still builds & simulates (a quick sanity check as they arrive).
- [ ] `java-basics-week2` reference branch ready on the repo (with the Gradle wrapper committed), so a stuck student can clone and run `./gradlew test`.
- [ ] The **Java exercise handout** (3–4 short problems) printed or linked for homework.
- [ ] Decide up front how deep to go on "what is an object" — for a room that's mostly new, spend the full concept slot there; for a stronger room, move faster and lean on the stretch track.
- [ ] Projector at ~18–20pt; a terminal visible for `./gradlew test` output.

**Most common friction this week:** the test command. `./gradlew test` on the first run downloads dependencies and can take a minute; on Windows it's `gradlew test` (no `./`). Warn them so a slow first run doesn't read as "broken."

---

## Board / screen setup (leave it up)

```
   A METHOD is a machine:  inputs ──►  [ deadband(value, band) ]  ──► output

   double raw = -0.03;                 // a VALUE (jitter near center)
   double out = RobotMath.deadband(raw, 0.1);   // CALL the method
   // out == 0.0

   A CLASS groups related methods.   An OBJECT is one built from a class.
   RobotMath holds only static methods → we never build one (no `new`).
```

---

## Minute-by-minute

### 0:00–0:10 — Warm-up & framing
- Recap Week 1 in one line: "You proved the tools work. Today you write real Java."
- Frame the week honestly for the mixed room: "Some of you have written Java; some haven't. That's expected. Everyone leaves with a class that works and a test that proves it."
- State the deliverable: *"a helper class you wrote, a green test, and it running on a joystick."*

### 0:10–0:20 — Concept + live demo
Teach only what the lab needs. Four ideas, each tied to a line of code:

1. **Values & types.** Robot values are almost always `double` (−1.0 to 1.0 for a stick, meters for distance). Show a variable.
2. **Methods.** "A method is a machine: inputs in, one value out." Point at `deadband(value, band)` on the board.
3. **Classes & objects.** A class groups methods; an object is one instance you make with `new`. **But** a *utility class* like `RobotMath` holds only `static` methods, so you call them on the class name and never say `new`. (This distinction confuses people — say it explicitly.)
4. **Why test?** "On a robot you can't `println` your way through everything. A test proves a method is right, once, forever."

**Live demo (~5 min):** open `RobotMath.java`, walk `deadband`. Then open `RobotMathTest.java`, run **`./gradlew test`**, show it pass. Change the expected value in one test to something wrong, re-run, show it **fail** — then fix it. That red→green moment is the point of the demo.

> **Why this matters:** it establishes testing as normal from the second week, before the code is complex enough to be scary.

### 0:20–0:55 — Hands-on lab
Students work from the handout. Core path everyone reaches; stretch for the fast.

**Core path:**
1. Open the starter; find `RobotMath` with `deadband` done as the worked example.
2. Implement `clamp` and `rotationsToMeters` themselves (signatures + docs provided).
3. Run `./gradlew test` until all pass.
4. In `Robot.java`, the joystick wiring is already there — simulate, set Teleoperated, push the left stick, watch raw vs. deadbanded values print.

**Verify per pair:** ✅ tests pass · ✅ they can explain what `clamp` returns for an out-of-range input · ✅ deadband visibly zeroes small stick values in sim.

**Stretch path:**
- Add a new method + its own test (e.g. `metersToRotations`, already in the pack as a model — have them write a *fresh* one like `feetToMeters`).
- Explore `record` and `enum` (a bridge to Week 3).
- Help a neighbor; be the one who explains "why no `new` for RobotMath."

**Floating priorities:** (1) test command issues (`./gradlew` vs `gradlew`, slow first run); (2) syntax errors — teach them to read the *first* compiler error, not the last; (3) the static-vs-instance confusion.

### 0:55–1:00 — Wrap & homework
- Confirm every student has a green test committed.
- Recap: "A method takes inputs and returns a value. A test proves it. `static` methods are called on the class."
- **Homework:** the Java exercise handout (3–4 short problems); optional: add one more tested method and push it.

---

## Anticipated student questions

**"Why `double` and not `int`?"** Robot values are rarely whole numbers — a stick at 0.42, a distance of 1.37 m. `double` holds decimals.

**"Why don't we write `new RobotMath()`?"** Because every method is `static` — it belongs to the class, not to an object. Utility classes are used by name: `RobotMath.clamp(...)`.

**"My test fails but my code looks right."** Read the failure message — JUnit prints expected vs. actual. Usually the math is off by a sign or an order-of-operations issue. Print the inputs if unsure.

**"`./gradlew` isn't found."** On Windows it's `gradlew test`. In VS Code's terminal, make sure you're in the project folder.

**"What's the difference between the demo's `deadband` and WPILib's?"** Almost none — WPILib ships `MathUtil.applyDeadband`. We write our own to learn how it works; either is fine on the real robot.

---

## Differentiation summary

| | New to programming | Knows Java already |
|---|---|---|
| **Focus** | What a method/return/`double` is; call an existing method; read a test result. | Write new methods + tests; `record`/`enum`; edge cases (what if `low > high`?). |
| **Success** | `clamp` + `rotationsToMeters` pass their tests. | A self-authored, tested method; helped a peer; can explain static vs instance. |

---

## Contingencies
- **Gradle/test trouble eats the room:** fall back to running the class from a `main` with `System.out.println` checks; make `./gradlew test` a homework follow-up. The *logic* is the objective, not the tool.
- **Robot/sim not needed for the core:** the tested helper is the real deliverable; the joystick step is the payoff, not a gate.
- **A pair is lost on syntax:** pair them tighter and give the fill-in-the-blank version (signatures done, bodies `// TODO`).

---
*Part of the FRC Java 12-Week Program (frc-java-12-week-program.md).*
