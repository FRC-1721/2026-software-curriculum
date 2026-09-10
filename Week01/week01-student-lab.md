# Week 1 — Student Lab
### Environment, tooling & "Hello, Robot"

**By the end of this hour you will have:** a robot project that builds, runs in a simulator, prints a message *you* wrote, and is saved to GitHub. No physical robot needed today.

**Work with your partner.** If you get stuck for more than a minute or two, raise a hand — don't sit stuck.

> **Two words you'll hear all hour**
> **Build** = turn your Java into something the robot/simulator can run.
> **Simulate** = run your code in a fake robot on your laptop.

---

## Step 0 — Check your tools (2 min)

1. Open **VS Code**.
2. Look at the top-right of the window for the **WPILib icon** — a red hexagon with a "W". If you see it, WPILib is installed. ✅
3. If you *don't* see it, tell a mentor now — you'll pair on a ready laptop.

> **The command palette is your friend.** Press **`Ctrl+Shift+P`** (Windows) or **`Cmd+Shift+P`** (Mac) and type `WPILib` to see every WPILib command. Clicking the "W" icon does the same thing.

---

## Step 1 — Create your project (5 min)

1. Open the command palette (`Ctrl/Cmd+Shift+P`) → type and choose **`WPILib: Create a new project`**.
2. Fill in the form:
   - **Project type:** `Template`
   - **Language:** `java`
   - **Base:** `Timed Robot`
   - **Base folder:** pick or create a folder for your FRC work.
   - **Project name:** `hello-robot`
   - **Team number:** *(ask your mentor — use the team's number)*
3. Click **Generate Project**, then **Yes (Current Window)** to open it.

**✅ Checkpoint 1:** the file explorer on the left shows folders like `src`, `gradle`, and files like `build.gradle`.

---

## Step 2 — Look around (3 min)

Open `src/main/java/frc/robot/Robot.java`. This is where you'll spend the whole course.

Find these two methods — you'll use them in a minute:

```java
@Override
public void teleopInit() {
  // runs ONCE when Teleop mode starts
}

@Override
public void teleopPeriodic() {
  // runs about 50 TIMES PER SECOND while in Teleop
}
```

> **The most important idea in robot code:**
> `Init` methods run **once**. `Periodic` methods run **~50 times a second** — that's the robot's heartbeat.

You can peek at `Main.java` too, but **don't change it** — it just starts the robot.

---

## Step 3 — Build it (3 min)

1. Command palette → **`WPILib: Build Robot Code`** (or click the "W" → Build).
2. Watch the terminal at the bottom. Wait for:

```
BUILD SUCCESSFUL
```

**✅ Checkpoint 2:** you see `BUILD SUCCESSFUL`. If you see `BUILD FAILED`, read the first red line and call a mentor.

---

## Step 4 — Make it say hello (5 min)

Inside `teleopInit()`, add **one line** with *your own name*:

```java
@Override
public void teleopInit() {
  System.out.println("Hello from Priya!");   // ← put YOUR name here
}
```

> `System.out.println(...)` prints a line of text. Whatever is inside the quotes shows up in the console.

Save the file (`Ctrl/Cmd+S`).

---

## Step 5 — Run it in the simulator (6 min)

1. Command palette → **`WPILib: Simulate Robot Code`**.
2. If it asks which extensions to use, check **`Sim GUI`** and click **OK**.
3. A **Simulation GUI** window opens — this is your pretend robot + Driver Station.
4. Find the **Robot State** panel. Click **Teleoperated**.
5. Look at the VS Code terminal (or the sim console). Your message appears:

```
Hello from Priya!
```

**✅ Checkpoint 3:** *your* message printed. 🎉 You just ran code you wrote on a (simulated) robot.

> **Try this (optional):** move your `println` line from `teleopInit()` into `teleopPeriodic()`, re-simulate, and enable Teleoperated again. Watch it print over and over and over. That's the 50-times-a-second loop! Now move it **back** to `teleopInit()` so it prints just once.

To stop the simulation, close the Sim GUI window or press the stop button in VS Code.

---

## Step 6 — Save your work to Git (10 min)

This is how the team shares code and how you never lose your work. Expect this step to feel fiddly the first time — that's normal.

1. Open a terminal in VS Code: **Terminal → New Terminal**.
2. Tell Git who you are (**one time only**, use your real name/email):
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "you@example.com"
   ```
3. Start tracking your project and make your first commit:
   ```bash
   git init
   git add .
   git commit -m "Week 1: Hello, Robot"
   ```
4. Connect to the team repo and push to **your own branch** (ask your mentor for the repo URL and your branch name `wk1/<yourname>`):
   ```bash
   git remote add origin <TEAM-REPO-URL>
   git branch -M wk1/yourname
   git push -u origin wk1/yourname
   ```

**✅ Checkpoint 4 (the finish line):** run `git log --oneline` and see your commit. If you pushed, your branch shows up on GitHub.

> If pushing to GitHub gives you an authentication error, don't panic — get the **local commit** done (steps 1–3). Pushing can be finished with a mentor or as homework.

---

## 🏁 You're done when…

- [ ] Your project shows `BUILD SUCCESSFUL`.
- [ ] *Your* message printed in the simulator.
- [ ] `git log` shows your first commit.

---

## ⭐ Stretch goals (if you finished early)

1. **Open a pull request.** On GitHub, open a PR from `wk1/yourname` into `main`. (Ask a mentor to walk through it — this is the real team workflow.)
2. **A useful loop.** In `teleopPeriodic()`, print a number that counts up, using a field at the top of the class:
   ```java
   private int loops = 0;

   @Override
   public void teleopPeriodic() {
     loops++;
     if (loops % 50 == 0) {              // every ~1 second
       System.out.println("Seconds enabled: " + loops / 50);
     }
   }
   ```
   Why the `if`? Because without it you'd print 50 times a second. This prints about once per second instead.
3. **Explore the Sim GUI.** Find the joysticks panel and the NetworkTables view. Poke around — you can't break anything.
4. **Be a helper.** Find a pair that's still working and get them to a green checkmark.

---

## Troubleshooting

| Problem | Try this |
|---|---|
| No "W" icon in VS Code | WPILib isn't installed on this machine — pair on a ready laptop; install on your own as homework. |
| `BUILD FAILED` | Read the **first** red line. Often a typo (missing `;` or `"`), or an old/2025 project — mentors can check the WPILib version. |
| Sim GUI never opens | Re-run **Simulate Robot Code** and make sure **Sim GUI** is checked. |
| Message doesn't print | Did you set Robot State to **Teleoperated**? Is the line in `teleopInit()` (not commented out)? Did you save the file? |
| `git push` rejected / asks for password | GitHub needs a token or SSH key. Get your local commit done first; finish push with a mentor. |
| "nothing to commit" | You already committed, or forgot `git add .`. Run `git status` to see. |

---

## Homework

1. If you used a shared laptop today, get **WPILib 2026** working on your **own** laptop — bring any problems to the team channel or next session.
2. Read WPILib **"Zero to Robot,"** steps 1–4.
3. *(Optional)* change your message and push a second commit.

---
*Part of the FRC Java 12-Week Program (frc-java-12-week-program.md). Pairs with the "Hello, Robot" starter code pack.*
