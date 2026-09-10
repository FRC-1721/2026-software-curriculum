# FRC Java Programming — 12-Week Onboarding Program

**Language:** Java only (WPILib 2026). No Python.
**Cadence:** 1 hour/week × 12 weeks.
**Audience:** Students new to FRC software development — *mixed* prior programming experience (some brand new, some already know Java basics).
**Hardware available:** WPILib simulation on every laptop + last year's **swerve** robot as a test bed.
**Draft status:** v1 for review — meant to be reworked and reorganized.

---

## 1. Philosophy & how to read this plan

The goal is not to make every student an expert in 12 hours — it's to make every student *fluent in the workflow*: they can clone the repo, build, deploy or simulate, read telemetry, find a problem in the data, and fix it. Everything is anchored to real robot code from day one; Java concepts are taught *because a subsystem needs them*, not in the abstract.

Three design constraints drive the whole plan:

- **One hour is short.** Each session is scoped tight (see the session template below) and leans on short between-session homework to hold momentum. Treat the 12 weeks as the *classroom* thread; expect the real depth to come during build-season lab time. Where a topic can't fit in 60 minutes, it's marked and given a homework bridge.
- **The group is mixed.** Every week has a **core path** (everyone must reach this) and a **stretch path** (for students who already know Java or move fast). New students are never blocked waiting; fast students are never bored. Pairing an experienced student with a newer one is the default seating.
- **Sim first, real robot as the reward.** We build and validate in WPILib simulation, then deploy to last year's swerve bot at three milestone moments (Weeks 6, 11, 12). Simulation keeps 100% of students hands-on even when the physical robot is in use by mechanical.

## 2. Software stack (install once, Week 1)

| Tool | Role | Notes for 2026 |
|---|---|---|
| **WPILib 2026 (VS Code)** | Core libraries + IDE + build (GradleRIO) | 2025 projects must be *imported* to be compatible. |
| **WPILib Simulation GUI** | Run/test robot code with no hardware | Primary lab environment weeks 1–5. |
| **AdvantageScope 2026** | Telemetry viewer & log analysis | The standard viewer. Shuffleboard/SmartDashboard are **deprecated** (removed in 2027) — we don't teach them. |
| **WPILib Epilogue** | Annotation-based telemetry (`@Logged`) | Modern 2026 way to publish data; replaces hand-wired SmartDashboard calls. |
| **AdvantageKit 2026** | Logging + deterministic replay | Introduced Weeks 10 (basics scope: logging + replay, not a full IO-layer rewrite). |
| **PathPlanner** | Autonomous path creation/following | Week 11. |
| **Vendordeps** | CTRE Phoenix 6 and/or REVLib | Match whatever last year's swerve bot actually uses — decide in Week 5. |
| **Git + GitHub** | Version control, code review | Team repo set up Week 1; PR workflow by Week 12. |
| **Driver Station / FRC Game Tools** | Enable/disable, control real robot | Needed from Week 6 (real-robot weeks). |

> **Mentor decision before Week 5:** confirm the swerve stack on last year's bot (CTRE TalonFX + Pigeon vs. REV Spark + NavX, and which swerve library/template — e.g. an AdvantageKit swerve template or YAGSL). The whole drivetrain thread keys off this.

## 3. Weekly session template (60 minutes)

| Time | Segment | Purpose |
|---|---|---|
| 0:00–0:10 | **Warm-up & recap** | Recap last week, answer homework questions, state today's one objective. |
| 0:10–0:20 | **Concept + live demo** | Mentor teaches the new idea and demos it live on the projector. |
| 0:20–0:55 | **Hands-on lab** | Students build. Core path for everyone; stretch path for fast movers. Mentors float. |
| 0:55–1:00 | **Commit, wrap & homework** | Commit to Git, state the deliverable, assign the short homework bridge. |

**Every week produces a committed deliverable** and a **checkpoint** a mentor can verify. That's the assessment — working code, not quizzes.

---

## 4. The 12 weeks

### Phase A — Foundations (Weeks 1–4)
*Get everyone tooled up, comfortable in Java, and able to build a working subsystem in simulation.*

---

#### Week 1 — Environment, tooling & "Hello, Robot"
- **Objective:** Every student can create a project, build it, run it in simulation, and commit to Git.
- **Concepts:** What FRC software is and where it runs (roboRIO, Driver Station); the WPILib VS Code environment; GradleRIO build; project anatomy (`Main.java`, `Robot.java`); the robot lifecycle (`robotInit` / `teleopPeriodic` / etc.); Git basics (clone, commit, push).
- **Live demo:** Build & simulate the template; open the Simulation GUI.
- **Core lab:** Create a new WPILib project from a template, build it, run it in the Simulation GUI, print a message in `teleopPeriodic`, make your first commit.
- **Stretch:** Set up the shared GitHub repo, write a `.gitignore`, create a personal branch, open a first pull request.
- **Deliverable / checkpoint:** Project builds and runs in sim on the student's own laptop; first commit pushed.
- **Homework:** Confirm the WPILib 2026 install works on your personal laptop; read WPILib "Zero to Robot."
- **Mentor prep:** Pre-install WPILib on shared laptops; create the GitHub org/repo; have a fallback laptop ready.
- **Common pitfalls:** WPILib version mismatch; JDK not found; forgetting to *import* a 2025 project.

#### Week 2 — Java for FRC, part 1: values, methods, objects
- **Objective:** Read and write basic Java grounded in robot values.
- **Concepts:** Primitive types (why robot values are usually `double`); variables; methods (parameters/return); classes vs. objects, `new`, fields; access modifiers; the idea of *units* and why mixing them bites you.
- **Live demo:** A tiny `RobotMath` class (unit conversions, clamping a value) written from scratch.
- **Core lab:** Write a small class with 2–3 methods (e.g. deadband, clamp, rotations→meters) and call it; read a joystick axis value and print it.
- **Stretch:** Write a JUnit test for the math class; explore `record` and `enum`.
- **Deliverable:** A committed, working helper class the student wrote.
- **Homework:** 3–4 short Java exercises (provided handout).
- **Differentiation note:** Weeks 2–3 are the Java on-ramp. Experienced students spend them on tests and design; new students spend them on fundamentals.

#### Week 3 — Java for FRC, part 2: control flow, enums, interfaces & lambdas
- **Objective:** Command the flow of a program and meet the OOP pieces command-based relies on.
- **Concepts:** `if`/`else`, boolean logic, loops; `enum` for robot states; inheritance & interfaces (why subsystems extend a base class); **lambdas & method references** (the backbone of command bindings); `Supplier`/`Consumer`.
- **Live demo:** A robot "state" enum + a method that reacts to it; a lambda passed as a `Runnable`.
- **Core lab:** Model a simple 3-state machine (e.g. `IDLE`/`INTAKING`/`SCORING`) and switch states from simulated button presses.
- **Stretch:** Refactor the state logic behind an interface; use functional interfaces (`DoubleSupplier`) the way commands do.
- **Deliverable:** A committed state-machine class driven by input.
- **Homework:** Implement one more state transition on your own.

#### Week 4 — The command-based framework
- **Objective:** Understand subsystems, commands, and the scheduler; build a controllable subsystem in sim.
- **Concepts:** `Subsystem` / `SubsystemBase`; `Command` and requirements; the `CommandScheduler` (use `CommandScheduler.getInstance().schedule(...)` — `Command.schedule()` is deprecated in 2026); default commands; `RobotContainer` and button bindings via `Trigger`/`CommandXboxController`; inline commands (`runOnce`, `run`) and simple compositions (sequence/parallel, decorators).
- **Live demo:** A simulated `Flywheel` or `Intake` subsystem with a default command and two button-bound commands.
- **Core lab:** Fill in a provided subsystem skeleton so a button spins it up and a default command holds it at rest; run in sim and watch the scheduler.
- **Stretch:** Compose two commands into a sequence; add a `Trigger` that fires on a sensor condition.
- **Deliverable:** A subsystem controlled from the controller in simulation.
- **Checkpoint (end of Phase A):** Student can independently create a subsystem + command and drive it in sim.

---

### Phase B — Real robot programming (Weeks 5–8)
*Motors, the swerve drivetrain, sensors, closed-loop control, and proper telemetry — culminating in driving the real robot.*

---

#### Week 5 — Motors, vendordeps & the swerve drivetrain (intro)
- **Objective:** Understand how motors are controlled and how a swerve drivetrain is structured.
- **Concepts:** Brushless motors & controllers (CTRE TalonFX/Phoenix 6, REV Spark/REVLib); installing **vendordeps**; the CAN bus; gear ratios & units; what swerve is (each module = drive motor + steer motor + absolute encoder); kinematics at a conceptual level.
- **Live demo:** Open last year's swerve code (or the chosen swerve template); walk the module structure; run swerve in simulation with the AdvantageScope field view.
- **Core lab:** Install the vendordeps; drive the *simulated* swerve bot with a controller; identify the four modules in code.
- **Stretch:** Read one module's code end-to-end; explain field-relative vs. robot-relative.
- **Deliverable:** Simulated swerve driving under student control.
- **Mentor prep:** Lock the swerve stack decision (CTRE vs REV, which template/library) and align the repo to it.

#### Week 6 — Driving the *real* robot: controllers, deadbands, safety ⭐
- **Objective:** Safely deploy to and drive last year's swerve bot.
- **Concepts:** `CommandXboxController`; joystick scaling & deadbands; slew-rate limiting; field-relative vs robot-relative teleop; **safety** (enable/disable, e-stop, brownout, space & spotters); the drive default command; deploying to the roboRIO via Driver Station.
- **Live demo:** First real deploy on the projector; safe enable/disable routine.
- **Core lab:** Deploy the team's code to the real bot; drive field-relative; tune deadband and max speed. Rotate driver/spotter/observer roles.
- **Stretch:** Implement slew-rate limiting and a "slow mode" speed scale.
- **Deliverable:** The team drives the real robot under their own code — **major milestone**.
- **Mentor prep:** Safety briefing, bumpers on, clear driving space, one spotter per run. Sim remains the fallback for students not at the bot.

#### Week 7 — Sensors & feedback: encoders, gyro, and closed-loop basics
- **Objective:** Read sensors and make a mechanism go to a setpoint.
- **Concepts:** Encoders (position & velocity); gyro/IMU and heading; odometry (concept only); open vs. closed loop; PID starting with **P**; the idea of feedforward for velocity; setpoints.
- **Live demo:** Live encoder/gyro values; adding P control to a mechanism and watching it converge (graph on screen).
- **Core lab:** Read and display encoder + gyro values; add P control to a test mechanism (sim or a bench subsystem) so it holds/reaches a setpoint.
- **Stretch:** Add I and D, plus a simple feedforward; mention SysId as the "proper" way to find gains.
- **Deliverable:** A mechanism that reaches a setpoint under closed-loop control.
- **Homework:** Predict what raising/lowering P will do; note it, then test next session.

#### Week 8 — Telemetry done right: Epilogue + AdvantageScope (live)
- **Objective:** Instrument the robot with modern telemetry and view it live.
- **Concepts:** NetworkTables (the pipe); **WPILib Epilogue** `@Logged` annotations (the modern 2026 way — no hand-wired dashboard calls); why we skip Shuffleboard/SmartDashboard; connecting AdvantageScope to sim and to the real robot; live line graphs, tables, and the 2D/3D field.
- **Live demo:** Annotate a subsystem with `@Logged`, deploy, open AdvantageScope, graph a value live.
- **Core lab:** Add `@Logged` to the drivetrain + one mechanism; open AdvantageScope; build and **save a layout** showing drive speed, a sensor value, and robot pose on the field.
- **Stretch:** Log a custom struct/pose; add a 3D mechanism visualization.
- **Deliverable:** A saved AdvantageScope layout showing live robot data.
- **Checkpoint (end of Phase B):** Student can add telemetry and read it live.

---

### Phase C — Telemetry-driven development & autonomy (Weeks 9–12)
*Analyze logs, add AdvantageKit logging + replay, program autonomous, and integrate everything.*

---

#### Week 9 — AdvantageScope deep dive: logs, scrubbing & debugging
- **Objective:** Use recorded logs to diagnose robot behavior after the fact.
- **Concepts:** On-robot data logging (`DataLogManager`, `.wpilog`); downloading logs from the roboRIO; timeline scrubbing; overlaying/comparing signals; spotting real problems (a module not tracking, current spikes, loop overruns); exporting.
- **Live demo:** Open a log with a *planted* fault and find it in the data.
- **Core lab:** Enable data logging, generate a log by driving (sim or real), download it, open it in AdvantageScope, and investigate a fault.
- **Stretch:** Correlate multiple signals to explain a behavior; find a loop-overrun event.
- **Deliverable:** A short written "log analysis" — what the data shows and why.
- **Homework:** Bring one question you'd want the logs to answer during a real match.

#### Week 10 — AdvantageKit: logging & deterministic replay (basics)
- **Objective:** Understand what AdvantageKit adds and get logging + replay working. *(Scoped to basics — not a full IO-layer rewrite.)*
- **Concepts:** What AdvantageKit is and why it exists (**deterministic replay**); the "logged robot" idea; inputs vs. outputs at a conceptual level; the replay workflow — replay a real run offline and add *new* logged values retroactively. Set up via an AdvantageKit template or the existing-project integration.
- **Live demo:** Replay a logged run and show a value that wasn't originally logged appearing after replay.
- **Core lab:** On a copy of the project, enable AdvantageKit logging, record a run, then **replay** it offline and reproduce the robot's state.
- **Stretch:** Implement a minimal IO interface for one subsystem (a first taste of the full pattern) — clearly optional.
- **Deliverable:** A successful replay of a logged run.
- **Mentor note:** Keep the ambition here to logging + replay. The full IO-abstraction architecture is flagged as a post-course topic.

#### Week 11 — Autonomous: command groups, odometry & PathPlanner ⭐
- **Objective:** Make the robot act on its own.
- **Concepts:** The autonomous period; command groups for auto; odometry-based movement; **PathPlanner** (paths, autos, event markers); selecting an auto with `SendableChooser`.
- **Live demo:** A simple command-group auto in sim, then a PathPlanner path following on the field view.
- **Core lab:** Build a simple auto (drive + one action) as a command group; create a PathPlanner path; run it in sim, then on the **real** swerve bot; verify path vs. actual pose in AdvantageScope.
- **Stretch:** Add event markers that trigger a mechanism mid-path; tune pose accuracy.
- **Deliverable:** A working autonomous routine, validated in sim and on the real robot.
- **Mentor prep:** Field space + safety for real auto runs; a known-good starting pose.

#### Week 12 — Capstone integration & showcase
- **Objective:** Combine teleop + auto + telemetry + logging into one working robot, and present it.
- **Concepts:** Integration; what "competition-ready" code looks like; Git code-review / PR workflow; season roadmap.
- **Live demo:** Mentor walks a clean, integrated reference project.
- **Core lab:** Complete a small integrated challenge on the real swerve bot — drive, run a scored auto, full telemetry + logging on; peer code review via pull requests.
- **Stretch:** Experienced students lead the code review and own the PR merge.
- **Deliverable:** Capstone demo + a short team retrospective + a clean repo on `main`.
- **Wrap — "beyond this course" roadmap:** vision & AprilTags; the full AdvantageKit IO-layer pattern; SysId characterization; advanced auto and state-space control.

---

## 5. Assessment & checkpoints

Assessment is competency-based, not test-based. Each week's committed deliverable is the evidence. Four phase gates:

- **End of Week 4:** Can independently build a subsystem + command and run it in sim.
- **End of Week 6:** Can safely deploy to and drive the real swerve robot.
- **End of Week 8:** Can add telemetry and read live data in AdvantageScope.
- **End of Week 12:** Can contribute integrated, reviewed code (teleop + auto + telemetry + logging).

A simple per-student checklist tracking these four gates is enough to see who needs extra lab time.

## 6. Risks & contingencies

- **60 minutes is tight.** Mitigation: strict session template, provided skeleton code, short homework bridges, and optional open-lab time during build season.
- **Real robot unavailable** (mechanical needs it, hardware fault): every real-robot week has a **sim fallback** so no session is lost.
- **Mixed pace pulls apart:** the core/stretch split plus experienced↔new pairing keeps both ends engaged.
- **Swerve stack uncertainty:** resolved by the Week-5 mentor decision; align the repo before Week 5.
- **Install friction on personal laptops:** shared pre-installed laptops as backup; Week-1 homework surfaces problems early.
- **Absences break continuity:** the Git repo is the source of truth; a student who misses a week pulls the reference branch and catches up from the committed deliverable.

## 7. What to prep before Week 1 (mentor checklist)

- GitHub org + team repo with a working starter project and reference branches per week.
- WPILib 2026 pre-installed on shared laptops; install guide for personal laptops.
- Last year's swerve bot confirmed operational; swerve stack (CTRE/REV + template) documented.
- Skeleton/starter code for Weeks 2, 4, 7, 10 (fill-in-the-blank labs).
- Java exercise handout (Weeks 2–3).
- A planted-fault log for Week 9 and a reference integrated project for Week 12.
- Driver Station / FRC Game Tools ready for real-robot weeks.

---
*Open items to decide during review: exact swerve library/template; whether to add a vision/AprilTag week by trimming elsewhere; how much build-season open-lab time supplements these 12 hours.*
