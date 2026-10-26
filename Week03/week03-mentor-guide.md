# Week 3 — Mentor Facilitation Guide
### Java for FRC, part 2: control flow, enums, interfaces & lambdas

**One-line goal for the hour:** every student builds a small state machine (`Superstructure`) using an `enum` and a `switch`, proves the transitions with a passing test, and drives it from controller buttons via lambdas.

**You are teaching:** a mixed group, still on the Java on-ramp. This week quietly introduces the exact building blocks of the command-based framework (enums, `switch`, `BooleanSupplier` lambdas), so it sets up Week 4.

**Format:** standard 60-minute template (10 · 10 · 35 · 5).

**Pairs with:** the Week 3 student lab and the `java-basics-week3` starter pack (verified: `RobotState`, `Superstructure`, and the JUnit tests compile clean and pass).

---

## Before class — mentor prep checklist
- [ ] Everyone finished Week 2 (a green `RobotMath` test). If not, that's a 2-minute fix while others settle.
- [ ] `java-basics-week3` reference branch ready (with the Gradle wrapper) for clone-and-run.
- [ ] Draw the state diagram on the board *before* class (below) — it does most of the teaching.
- [ ] Decide your button mapping for the sim demo: **A = intake, B = score, Y = stop** (matches the starter).

**Most common friction:** confusing `==` (comparison) with `=` (assignment), and forgetting that `switch` cases need `break`. Call both out during the demo.

---

## Board / screen setup (draw this first, leave it up)

```
        intake (A)            score (B)
  IDLE ───────────► INTAKING ───────────► SCORING
   ▲                                         │
   │              score (B) — "done scoring" │
   └─────────────────────────────────────────┘

   STOP (Y) from ANY state ───► IDLE   (and beats any other button)
```

The whole lesson is: *turn this picture into an `enum` plus a `switch`.*

---

## Minute-by-minute

### 0:00–0:10 — Warm-up & framing
- Recap Week 2: "You wrote and tested methods. Today those skills build something that *makes decisions*."
- Point at the state diagram: "A robot is usually in one of a few states. Code has to move between them based on input. That's a state machine, and you already know almost everything you need to build one."
- Deliverable: *"a working state machine, a green test, driven by buttons in the sim."*

### 0:10–0:20 — Concept + live demo
Four ideas, each on the board:

1. **Control flow.** `if`/`else` chooses; `boolean` is true/false. Show `if (stopButton) { ... }`. Warn: **`==` compares, `=` assigns** — mixing them is the classic bug.
2. **`enum`.** "Instead of 0/1/2, we name the states: `IDLE, INTAKING, SCORING`. Now illegal states can't even be written." Show `RobotState`.
3. **`switch`.** The clean way to branch on an enum. Every `case` ends with `break` (or you 'fall through' to the next — usually a bug). Show `Superstructure.update`.
4. **Lambdas / functional interfaces.** `driver::getAButton` is a `BooleanSupplier` — "a value we ask for each loop." Foreshadow: "This is exactly how Week 4's button bindings work."

**Live demo (~5 min):** walk `RobotState` (note it even carries a `label()` — enums can hold data). Walk `Superstructure.update`'s `switch`. Run **`./gradlew test`**, show green. Then break one transition (e.g. delete a `break` or change a target state), re-run, show the failing test catch it, and fix it.

> **Why this matters:** the failing test *localizes* the bug for them — the lesson is "tests tell you exactly which rule you broke."

### 0:20–0:55 — Hands-on lab

**Core path:**
1. Complete `RobotState` (the three enum values).
2. Complete the `switch` in `Superstructure.update(...)` to match the board diagram.
3. Run `./gradlew test` until all transition tests pass.
4. In `Robot.java`, buttons are already wired as lambdas — simulate, set Teleoperated, and press A/B/Y in the sim's joystick panel; watch the state print as it changes.

**Verify per pair:** ✅ tests pass · ✅ pressing A from IDLE prints `-> Intaking` · ✅ they can point to where STOP is handled and explain why it's checked first.

**Stretch path:**
- Add a fourth state (e.g. `EJECTING`) with its own transition and a new test.
- Use the `BooleanSupplier` overload directly with a lambda that combines buttons (e.g. "score only if a bumper is also held").
- Extract an interface (e.g. `interface Updatable { RobotState update(...); }`) and discuss why command-based subsystems lean on interfaces/inheritance.

**Floating priorities:** (1) `==` vs `=`; (2) missing `break` / fall-through; (3) comparing enums — `state == RobotState.IDLE` is correct and clean.

### 0:55–1:00 — Wrap & homework
- Confirm every student's transition tests are green and committed.
- Recap: "Name your states with an `enum`, branch with a `switch`, and feed inputs as suppliers. That's the skeleton of every subsystem you'll write."
- **Homework:** add one more transition (or state) and a test for it; read ahead: "next week these ideas become Subsystems and Commands."

---

## Anticipated student questions

**"Why an enum instead of numbers?"** Names read like English and the compiler stops you from using a state that doesn't exist. `state = RobotState.SCORING;` can't be mistyped as `3`.

**"My state never changes."** Two usual causes: you compared with `=` instead of `==` somewhere, or a `switch case` is missing its `break` and falling through. Check both.

**"What's `driver::getAButton`?"** A method reference — shorthand for the lambda `() -> driver.getAButton()`. It's a `BooleanSupplier`: something the machine calls each loop to get true/false.

**"Why check STOP before the switch?"** Because STOP should win from *any* state. Handling it first, before the per-state logic, keeps that rule in one place.

**"Do real robots use this?"** Yes — subsystems constantly track state this way, and Week 4's command framework is built on exactly these pieces (enums, suppliers, small decision methods).

---

## Differentiation summary

| | New to programming | Knows Java already |
|---|---|---|
| **Focus** | `if`/`switch`/`enum` mechanics; make the given diagram work. | New states, interfaces, combined-button lambdas, edge cases. |
| **Success** | Transition tests pass; buttons move the state in sim. | An added state + test; can explain functional interfaces to a peer. |

---

## Contingencies
- **Gradle/test trouble:** run a small `main` that calls `update(...)` with hard-coded booleans and prints the state, as a fallback for the tests.
- **Sim/joystick confusion:** the tested state machine is the real deliverable; the button demo is the payoff.
- **A pair is stuck on the `switch`:** hand them the fill-in-the-blank version (cases present, bodies `// TODO`) and have them do one transition at a time, re-running the test after each.

---
*Part of the FRC Java 12-Week Program (frc-java-12-week-program.md).*
