# Week 3 — Student Lab
### Java for FRC, part 2: control flow, enums, interfaces & lambdas

**By the end of this hour you will have:** a working state machine you built with an `enum` and a `switch`, a passing test that proves every transition, and the machine responding to controller buttons in the simulator.

**Work with your partner.** Stuck for more than a minute? Raise a hand.

> **Words this week**
> **enum** = a fixed set of named values (our robot's states).
> **switch** = a clean way to do something different for each value.
> **lambda** = a tiny function you can pass around (like a button's on/off).

---

## The machine you're building

```
        intake (A)            score (B)
  IDLE ───────────► INTAKING ───────────► SCORING
   ▲                                         │
   │              score (B) — done scoring   │
   └─────────────────────────────────────────┘

  STOP (Y) from ANY state ───► IDLE   (and beats any other button)
```

---

## Step 0 — Get the starter (2 min)

Open the `java-basics-week3` pack. Find:
- `superstructure/RobotState.java` — the states (an `enum`).
- `superstructure/Superstructure.java` — the state machine (a `switch`).
- `src/test/.../SuperstructureTest.java` — the tests.

---

## Step 1 — Name the states (4 min)

Open `RobotState.java`. It's an `enum` — a fixed list of names. Make sure it has the three states:

```java
public enum RobotState {
  IDLE("Idle"),
  INTAKING("Intaking"),
  SCORING("Scoring");
  // (the label in quotes is a friendly name — enums can hold data!)
}
```

> **Why an enum?** So the rest of the code says `RobotState.SCORING` instead of a mystery number. You literally cannot mistype a state that doesn't exist.

---

## Step 2 — Build the transitions (10 min)

Open `Superstructure.java`. Complete the `switch` so it matches the diagram:

```java
public RobotState update(boolean intakeButton, boolean scoreButton, boolean stopButton) {
  if (stopButton) {          // STOP wins from any state
    state = RobotState.IDLE;
    return state;
  }

  switch (state) {
    case IDLE:
      if (intakeButton) { state = RobotState.INTAKING; }
      break;                 // ← don't forget break!
    case INTAKING:
      if (scoreButton) { state = RobotState.SCORING; }
      break;
    case SCORING:
      if (scoreButton) { state = RobotState.IDLE; }
      break;
    default:
      break;
  }
  return state;
}
```

> **Two classic bugs to avoid:**
> `==` compares (`state == RobotState.IDLE`), `=` assigns. Don't mix them.
> Every `case` needs a `break`, or it "falls through" into the next one.

---

## Step 3 — Run the tests (4 min)

```bash
./gradlew test
```
*(Windows: `gradlew test`.)*

**✅ Checkpoint 1:** all transition tests pass. A failure names the exact transition that's wrong — read it and fix that one rule.

---

## Step 4 — Drive it with buttons (8 min)

Open `Robot.java`. The buttons are already wired as **lambdas** — the same shape you'll use in Week 4:

```java
RobotState state = superstructure.update(
    driver::getAButton,   // A = intake
    driver::getBButton,   // B = score
    driver::getYButton);  // Y = stop
```

> `driver::getAButton` is a `BooleanSupplier` — the machine asks it "pressed?" every loop.

1. `WPILib: Simulate Robot Code` → **Sim GUI** → set **Teleoperated**.
2. In the sim's **Joysticks** panel, map a controller or click the buttons for A/B/Y.
3. Press **A** → console prints `-> Intaking`. Press **B** → `-> Scoring`. Press **B** again → `-> Idle`. Press **Y** anytime → `-> Idle`.

**✅ Checkpoint 2:** the printed state follows the diagram as you press buttons.

---

## 🏁 You're done when…
- [ ] `./gradlew test` passes.
- [ ] Pressing A/B/Y walks the state through the diagram in the sim.
- [ ] You can point to where STOP is handled and say why it's checked first.

---

## ⭐ Stretch goals
1. **Add a state.** Add `EJECTING` to the enum, a transition to reach it (say, holding intake while in SCORING), and a test for it. Re-run `./gradlew test`.
2. **Combine buttons with a lambda.** Use the `BooleanSupplier` overload with a lambda like `() -> driver.getBButton() && driver.getXButton()` so scoring needs two buttons held together.
3. **Make an interface.** Extract `interface Updatable { RobotState update(boolean i, boolean s, boolean stop); }` and have `Superstructure` implement it. Ask a mentor why command-based subsystems rely on interfaces.
4. **Be a helper.** Get a neighbor's transitions to green.

---

## Troubleshooting

| Problem | Try this |
|---|---|
| State never changes | You probably used `=` where you meant `==`, or a `case` is missing `break`. |
| Test fails on one transition | The message names it — fix just that `case`. |
| `-> Idle` prints constantly | You're printing every loop — print only when `state != lastState` (the starter already does this; check you didn't move the print). |
| Buttons do nothing in sim | Set Robot State to **Teleoperated**, and make sure the Joysticks panel has a controller mapped. |
| `cannot find symbol RobotState` | Check the `import frc.robot.superstructure.RobotState;` and the `package` lines. |

---

## Homework
1. Add one more transition (or state) to your machine **and a test for it**.
2. Read-ahead: next week these exact ideas become **Subsystems** and **Commands**.

---
*Part of the FRC Java 12-Week Program (frc-java-12-week-program.md). Pairs with the `java-basics-week3` starter pack.*
