# Week 2 — Student Lab
### Java for FRC, part 1: values, methods, objects

**By the end of this hour you will have:** a Java helper class you wrote, a passing test that proves it works, and that helper running on a live joystick in the simulator.

**Work with your partner.** Stuck for more than a minute? Raise a hand.

> **Three words this week**
> **Value** = a piece of data, usually a `double` (like `0.42`).
> **Method** = a little machine: inputs in, one value out.
> **Test** = code that proves a method is correct.

---

## Step 0 — Get the starter (3 min)

Open the `java-basics-week2` pack (or your Week 2 project). Find:
- `src/main/java/frc/robot/util/RobotMath.java` — the class you'll work in.
- `src/test/java/frc/robot/util/RobotMathTest.java` — the tests.

Open `RobotMath.java` and read `deadband(...)` — it's done for you as a worked example.

---

## Step 1 — Read a method (4 min)

```java
public static double deadband(double value, double deadband) {
  if (Math.abs(value) > deadband) {
    if (value > 0.0) {
      return (value - deadband) / (1.0 - deadband);
    } else {
      return (value + deadband) / (1.0 - deadband);
    }
  }
  return 0.0;
}
```

- `public static double` — anyone can call it, it belongs to the class (not an object), and it returns a `double`.
- `(double value, double deadband)` — its two **inputs**.
- `return ...` — the **output**.

> **Why "static"?** `RobotMath` is a *utility class* — just a bag of methods. You call them on the class name: `RobotMath.deadband(0.5, 0.1)`. You never write `new RobotMath()`.

---

## Step 2 — Write `clamp` (7 min)

Find the `clamp` method. Its job: keep a value inside a range. Fill in the body so:
- `clamp(5.0, 0.0, 1.0)` → `1.0`  (too high → snap to the top)
- `clamp(-5.0, 0.0, 1.0)` → `0.0`  (too low → snap to the bottom)
- `clamp(0.5, 0.0, 1.0)` → `0.5`  (already inside → unchanged)

> **Hint:** `Math.min(a, b)` returns the smaller; `Math.max(a, b)` the larger. You can nest them.

---

## Step 3 — Write `rotationsToMeters` (5 min)

A wheel turning one full rotation rolls a distance equal to its circumference, `π × diameter`. Fill in the body so:
- `rotationsToMeters(1.0, 0.1016)` → about `0.319` (a 4-inch wheel, one turn)

> **Hint:** `Math.PI` is π.

---

## Step 4 — Run the tests (4 min)

In the VS Code terminal:

```bash
./gradlew test
```
*(Windows: `gradlew test`. The first run downloads things and can take a minute — that's normal.)*

**✅ Checkpoint 1:** the output ends with `BUILD SUCCESSFUL` and no failing tests. If a test fails, read the message — it shows what it **expected** vs. what your method **returned** — and fix the math.

---

## Step 5 — Use it on a real joystick (6 min)

Open `Robot.java`. It already reads the left-stick Y value and prints it before and after your `deadband`:

```java
double rawY = driver.getLeftY();
double commanded = RobotMath.deadband(rawY, 0.1);
```

1. `WPILib: Simulate Robot Code` → check **Sim GUI** → OK.
2. Set Robot State to **Teleoperated**.
3. In the sim's **Joysticks** panel, drag the left-stick Y axis a tiny bit — the raw value moves but `after deadband` stays `0.00`. Push it far — both move together.

**✅ Checkpoint 2:** you can see the deadband removing small jitter near center.

---

## 🏁 You're done when…
- [ ] `./gradlew test` passes with your `clamp` and `rotationsToMeters`.
- [ ] You can explain, in a sentence, what `clamp` does.
- [ ] The joystick demo shows the dead zone working.

---

## ⭐ Stretch goals
1. **Write a new method + test.** Add `feetToMeters(double feet)` (1 foot = 0.3048 m) to `RobotMath`, then add a test for it in `RobotMathTest` modeled on the others. Run `./gradlew test`.
2. **Edge cases.** What should `clamp` do if someone calls `clamp(0.5, 1.0, 0.0)` (low > high)? Try it, see what happens, and talk with a mentor about how you'd guard against it.
3. **Meet `record` and `enum`.** Ask a mentor to show you a one-line `record` — it's a preview of Week 3.
4. **Be a helper.** Get a neighbor's tests to green.

---

## Troubleshooting

| Problem | Try this |
|---|---|
| `./gradlew: not found` | On Windows type `gradlew test`. Make sure the terminal is in the project folder. |
| First `test` run hangs a while | Normal on the first run — it's downloading dependencies. Give it a minute. |
| A test fails | Read the expected-vs-actual in the message. Check signs and parentheses. Print the inputs if unsure. |
| `cannot find symbol RobotMath` | Check the `package` line and that you're calling `RobotMath.clamp(...)` (capital R, on the class). |
| Red squiggles everywhere | Usually a missing `;` or `}`. Fix the **first** error the editor lists, then recheck. |

---

## Homework
1. Complete the **Java exercise handout** (short problems on methods and `double` math).
2. *(Optional)* add one more tested method to `RobotMath` and push a commit.

---
*Part of the FRC Java 12-Week Program (frc-java-12-week-program.md). Pairs with the `java-basics-week2` starter pack.*
