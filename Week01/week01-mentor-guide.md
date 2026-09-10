# Week 1 — Mentor Facilitation Guide
### Environment, tooling & "Hello, Robot"

**One-line goal for the hour:** every student leaves with a WPILib 2026 project that *builds, runs in simulation, prints a message they wrote, and is committed to Git.*

**You are teaching:** a mixed group (some new to programming, some know Java). Week 1 is almost entirely *tooling and workflow* — no real Java yet — so it's the great equalizer. Lean on your experienced students as co-teachers here.

**Format:** the standard 60-minute template (10 warm-up · 10 concept+demo · 35 lab · 5 wrap).

---

## Before class — mentor prep checklist

Do these *before* students arrive. Week 1 lives or dies on install friction.

- [ ] **WPILib 2026 pre-installed** on every shared laptop (the offline installer is large — don't rely on room Wi-Fi for 15 simultaneous downloads).
- [ ] **GitHub org + team repo created**, with you as owner. Have a `week-01-reference` branch ready containing the finished "Hello, Robot" project (from the starter code pack).
- [ ] **One fallback laptop** fully working, in case a student's environment won't cooperate — they pair on it rather than lose the hour.
- [ ] **Projector tested** with WPILib VS Code at a readable font size (Ctrl/Cmd-+ a few times; aim for ~18–20pt editor font).
- [ ] **Print or share the student lab handout** (one per student or on-screen).
- [ ] **Know your GitHub invite flow** — decide now whether students push to branches on the team repo or fork. Recommended for beginners: everyone gets write access and pushes to a personal branch `wk1/<name>`.
- [ ] Have the **WPILib command cheat sheet** visible (see the starter pack).

**The single most common failure:** a student's laptop has an old WPILib, a system JDK conflict, or a 2025 project that won't open. Budget for it — that's what the fallback laptop and experienced-student pairing are for.

---

## Board / screen setup (draw this once, leave it up)

Draw this diagram on the whiteboard at the start and refer back to it all hour:

```
   YOUR LAPTOP                         THE ROBOT
 ┌──────────────┐    deploy / USB    ┌──────────────┐
 │  VS Code +   │  ───────────────►  │   roboRIO    │
 │  WPILib      │                    │ (runs your   │
 │  (writes &   │  ◄───────────────  │  Java code)  │
 │  builds code)│    telemetry       └──────┬───────┘
 └──────┬───────┘                           │ CAN bus
        │ Git                          motors, sensors
        ▼
   ┌─────────┐     ┌───────────────────────────────┐
   │ GitHub  │     │  ...OR: SIMULATION on laptop   │
   │ (repo)  │     │  (no robot needed — this week) │
   └─────────┘     └───────────────────────────────┘
```

The message: *the code you write on your laptop eventually runs on the roboRIO — but this week we run it in a simulator instead, so nobody needs the physical robot.*

---

## Minute-by-minute

### 0:00–0:10 — Warm-up & framing

**Say (roughly):**
> "Welcome to FRC software. Over 12 weeks you're going to go from this — an empty project — to writing code that drives a real swerve robot and even runs on its own. Today is the least glamorous and one of the most important weeks: we set up your tools and prove the whole loop works. By the end of the hour, code *you* wrote will be running."

**Do:**
- Take a quick show of hands: "Who has written any code before? Any Java?" Use this to seat pairs — put one experienced student with one new student at each station.
- Point at the board diagram. Explain the three places code lives: **your editor**, **the robot (roboRIO)**, and **GitHub**. Say that today the robot is replaced by a **simulator**.
- State today's one deliverable in plain terms: *"build, simulate, print something you wrote, commit it."*

**Watch for:** don't over-explain here. Ten minutes, then hands on keyboards.

---

### 0:10–0:20 — Concept + live demo

Keep this tight and *show*, don't lecture. Project your screen. Narrate every click — beginners lose you when the mouse moves silently.

**Talking points (the four things they need to know):**

1. **What FRC code is.** "It's a Java program that follows a fixed rhythm. The robot library calls certain methods for you, over and over — about 50 times a second." Introduce the **robot lifecycle**: `robotPeriodic`, and the mode methods `disabled`, `autonomous`, `teleop`, `test`, each with an `Init` (runs once when the mode starts) and a `Periodic` (runs ~every 20 ms while in that mode). Write on the board: **`Init` = once, `Periodic` = ~50×/second.**

2. **The tools.** VS Code is the editor. The **WPILib extension** adds the FRC commands (the little red hexagon "W" in the top-right, or the command palette). **GradleRIO** is the build system that turns your Java into something the robot or simulator can run — they won't touch it directly today.

3. **The project anatomy.** Open the reference project and show:
   - `src/main/java/frc/robot/Robot.java` — "this is where you'll live."
   - `Main.java` — "don't touch it; it just starts the robot."
   - `build.gradle` — "the build recipe; we leave it alone this week."
   - `.gitignore`, `vendordeps/` — mention in one line each.

4. **Simulation.** "We can run all of this with no robot at all. The simulator opens a window that pretends to be a robot and Driver Station."

**Live demo (do it yourself, on the projector, ~5 min):**

1. `WPILib: Create a new project` → **Template → java → Timed Robot** → pick a folder → project name `hello-robot` → team number (use your team's, or `9999` for demo) → **Generate**.
2. Open `Robot.java`. Point out `teleopInit()` and `teleopPeriodic()`.
3. Add one line to `teleopInit()`:
   ```java
   System.out.println("Hello, Robot! — Coach demo");
   ```
4. `WPILib: Simulate Robot Code` → check **Sim GUI** → **OK**.
5. In the Sim GUI, set **Robot State** to **Teleoperated**. Show the message appear in the VS Code terminal / console.
6. **The teachable twist:** move the same `println` into `teleopPeriodic()`, re-simulate, enable Teleop, and let it *flood* the console. "See how it prints forever? That's the 50-times-a-second loop. Remember that — printing in `Periodic` is how you accidentally spam yourself." Move it back to `teleopInit`.

> **Why this demo matters:** it plants the single most important mental model of the whole course — the periodic loop — in a way they *see*, not just hear.

---

### 0:20–0:55 — Hands-on lab (the core of the hour)

Students now do it themselves from the **student lab handout**. You and co-mentors float. The lab has explicit checkpoints; your job is to get every pair past each one.

**Core path (everyone must reach):**
1. Verify WPILib 2026 is installed (open VS Code, see the "W" icon).
2. Create a new project from the **Timed Robot** template.
3. Build the project (`WPILib: Build Robot Code`) — see `BUILD SUCCESSFUL`.
4. Add a `System.out.println("Hello from <name>!")` to `teleopInit()`.
5. Simulate, set state to Teleoperated, see their message.
6. `git init`, first commit, push to their branch `wk1/<name>`.

**Checkpoints to verify per pair** (walk around and physically confirm):
- ✅ `BUILD SUCCESSFUL` appeared.
- ✅ Their *own* message printed in the sim console.
- ✅ `git log` shows their first commit; branch is pushed.

**Stretch path (for students who fly through):**
- Write a proper `.gitignore` review and open a **pull request** from `wk1/<name>` into `main`.
- Print a *countdown* in `teleopPeriodic` using a counter variable (a first taste of the loop being useful, not just noisy).
- Explore the Sim GUI: joystick panel, the timing, NetworkTables view.
- Help a neighbor who's stuck (make this explicit and praised — it's how mixed groups work).

**Floating-mentor priorities, in order:**
1. Anyone whose project won't **generate** or **build** — unblock first (usually JDK/version). Fall back to the shared laptop.
2. Anyone who can't **simulate** — usually forgot to check the Sim GUI extension, or didn't set robot state to Teleoperated.
3. **Git** — expect this to eat time. Auth (token/SSH), first `git config user.name/email`, and "nothing to commit" confusion are the usual snags.

---

### 0:55–1:00 — Commit, wrap & homework

**Do:**
- Make sure *every* student has pushed at least one commit. If someone hasn't, that's your follow-up before next week.
- Recap the mental model out loud: "Editor → build → simulate → commit. `Init` once, `Periodic` fifty times a second. Next week we start writing actual Java."
- Assign homework.

**Homework (state clearly, it's on the handout):**
1. Make sure the WPILib 2026 install works on your **personal** laptop (if you used a shared one today) — bring problems to next week or the team channel.
2. Read WPILib **"Zero to Robot"** (steps 1–4).
3. Optional stretch: change your printed message and push a second commit.

---

## Anticipated student questions (with answers)

**"Why Java and not Python / C++?"**
> Our whole codebase and last year's robot are in Java, and it's the most common FRC language with the most support. You'll be able to read real team code from day one.

**"Do I need the robot to test my code?"**
> Not this week — the simulator does it. We'll deploy to last year's swerve bot in Week 6.

**"My build failed / it's red everywhere."**
> Almost always one of: wrong WPILib version, a JDK conflict, or an un-imported 2025 project. Let's check the version first. (Fall back to the shared laptop if it drags past ~5 min.)

**"What's the difference between `Init` and `Periodic`?"**
> `teleopInit` runs *once*, the instant Teleop starts. `teleopPeriodic` runs about every 20 milliseconds — 50 times a second — the whole time you're in Teleop. That loop is the heartbeat of robot code.

**"Why is my print statement showing thousands of times?"**
> Because you put it in a `Periodic` method — it runs 50×/second. Put one-time messages in an `Init` method.

**"Do I have to use Git? It's confusing."**
> Yes — it's how the team shares code and how you recover when something breaks. It feels clunky for a week, then it's second nature. Today just get one commit pushed.

**"What team number do I put?"**
> Use ours: `<your team #>`. For pure simulation it doesn't really matter, but set it correctly out of habit.

---

## Differentiation summary

| | New to programming | Knows Java already |
|---|---|---|
| **Focus** | The *workflow*: generate, build, simulate, commit. Don't worry about the code yet. | The *tooling depth*: branches, PRs, `.gitignore`, the Sim GUI internals. |
| **Success looks like** | Their message printed; one commit pushed. | A merged/open PR; a working `teleopPeriodic` counter; helped a neighbor. |
| **If they finish early** | Pair them as a "tester" reading the handout aloud to a partner. | Give the stretch tasks; recruit as a floating helper. |

---

## Contingencies

- **Wi-Fi/install meltdown:** shared pre-installed laptops are the plan; nobody installs during class if it's risky. Personal-laptop installs are homework.
- **Physical robot busy / absent:** irrelevant this week — 100% simulation.
- **Git auth chaos:** if GitHub auth eats the room, have everyone commit *locally* (`git init` + `git commit`) as the checkpoint, and handle push/auth as a between-session task.
- **A pair falls behind:** the goal is *build + simulate + local commit.* Pushing can slip to homework without breaking the sequence.

---
*Pairs with: the Week 1 student lab handout and the "Hello, Robot" starter code pack. Part of the FRC Java 12-Week Program (frc-java-12-week-program.md).*
