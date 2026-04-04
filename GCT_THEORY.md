<p align="center">
  <strong>G C T</strong><br>
  <em>General Constraint Theory</em>
</p>

<p align="center">
  A universal framework for systematic improvement of any system.<br>
  Software. Business. Body. Mind. Life.<br><br>
  <strong>Created by Solfridr Deas -- March 2026</strong>
</p>

---

## What is GCT?

> **A system does not fail because it lacks capability. It stagnates because it has unexamined constraints.**

Most people improve systems by adding -- more features, more effort, more resources. GCT says the opposite. You improve a system by removing what limits it.

Resolve one constraint, the system reveals the next layer. That is not failure. That is growth.

**GCT is not a methodology you apply sometimes. It is a lens you see through always.**

---

## Quick Start

```
1. IDENTIFY    What is the one thing limiting this system right now?
2. ANALYZE     Why does that constraint exist? What type is it?
3. RESOLVE     Remove it, replace it, or route around it.
4. ITERATE     Look again. The next constraint is already visible.
```

That is the entire framework. Everything below is depth.

---

## Table of Contents

- [The Four Moves](#the-four-moves)
- [The Constraint Taxonomy (T1-T6)](#the-constraint-taxonomy)
- [The Three Resolutions](#the-three-resolutions)
- [Meta-GCT -- Auditing the Audit](#meta-gct)
- [LBT -- Logical Boundary Theory](#lbt--logical-boundary-theory)
- [Applied Example: The Solm Audit](#applied-example-the-solm-audit)
- [Practical Checklist](#practical-checklist)
- [GCT in One Breath](#gct-in-one-breath)

---

## The Four Moves

GCT operates through four moves. They are sequential. Skipping moves causes damage.

### Move 1 -- Identify

Find the constraint. Not the symptom. The actual limiting factor.

Systems hide their real constraints behind surface-level problems. A slow application is a symptom. The constraint might be an O(n^2) loop, a missing cache, a decision made in week one, or the developer's incomplete understanding of the runtime.

**The question:** *What is the single thing that, if removed, would allow this system to move faster toward its goal?*

**Practice identification with five whys:**

| Layer | Question |
|-------|----------|
| Surface | The app is slow. *Why?* |
| Symptom | The home screen takes 3 seconds. *Why?* |
| Mechanism | It recalculates every data point on every render. *Why?* |
| Gap | There is no caching layer. *Why?* |
| **Constraint** | **Nobody identified the need because performance was fine with 10 entries.** |

The constraint is not "the app is slow." The constraint is "the system has no mechanism to distinguish data that changed from data that did not." That distinction tells you *what to build*, not just *what to fix*.

**Common mistakes:**
- Naming the symptom instead of the constraint
- Naming too many constraints at once -- there is always one *binding* constraint, the one that matters most right now
- Naming constraints you cannot affect

### Move 2 -- Analyze

Understand the constraint completely before touching it.

Every constraint exists for a reason. Some protect the system. Some are accidents of history. Some are genuine environmental limits. You must know which before you act.

**Five analysis questions:**

1. Why does this constraint exist?
2. What type is it? *(see taxonomy below)*
3. What does the system look like with it removed?
4. What new constraints would removal reveal?
5. Is it **load-bearing** (holding something together) or **artificial** (an assumption never tested)?

**Never remove a constraint you do not understand. Removing the wrong constraint can collapse the system.**

### Move 3 -- Resolve

Act on the analysis. Three strategies exist -- see [The Three Resolutions](#the-three-resolutions).

**The rule of minimum effective intervention:** Always ask: *what is the smallest change that resolves this constraint?* The best resolution is often one line of code, one config change, one reframe. If your resolution requires rewriting the system, you probably misidentified the constraint.

### Move 4 -- Iterate

The moment one constraint is resolved, return to Move 1.

This is not optional. This is the engine. A system that resolves one constraint and stops has improved once. A system that resolves constraints continuously is a *self-improving system*.

You do not plan all the moves. You plan the next move, execute it, and then see what the system reveals.

---

## The Constraint Taxonomy

Six types. Classify before acting.

| Type | Name | What it is | Resolved by | Watch for |
|------|------|------------|-------------|-----------|
| **T1** | **Resource** | Limited compute, money, time, credits, storage | Efficiency, caching, cheaper alternatives, batching | Resource constraints that are really knowledge constraints in disguise. "Not enough budget" sometimes means "don't know where budget has most impact." |
| **T2** | **Knowledge** | Missing data, incomplete understanding, unvalidated assumptions | Data collection, measurement, feedback loops, learning systems | Often invisible -- the system doesn't know what it doesn't know. Best question: *"What would change our approach if we knew it?"* |
| **T3** | **Technical** | Architecture can't do what it needs to. Missing integrations, wrong data structures, performance limits | Building new capability, refactoring, integrating tools, redesigning flow | Technical constraints that are really philosophical. "Can't do X" sometimes means "haven't decided it should do X." |
| **T4** | **Market** | External limits: saturation, low engagement, wrong timing, competitive pressure | Expansion, repositioning, timing optimization, niche pivoting | Market constraints that are really knowledge constraints. "Market isn't responding" might mean "don't understand what the market wants." |
| **T5** | **Human** | Attention limits, approval bottlenecks, manual steps, decision fatigue, habit patterns | Automation, better interfaces, smarter defaults, delegation | Most sensitive to resolve because they involve changing behavior. Best resolution makes the right behavior the easiest behavior. |
| **T6** | **Philosophical** | Assumptions about what the system *is*, what it is *for*, what success *means* | Reframing. Questioning the premise. Expanding the definition. | Often feel like truths rather than constraints. "That's just how it is" is a philosophical constraint wearing the mask of reality. |

**Type 6 is the deepest.** Resolving a philosophical constraint can make an entire layer of other constraints disappear at once. Assuming a training tracker is "only a tracker" is T6. Reframe it as an intelligence system that happens to track training, and capabilities that a "tracker" could never develop become immediately buildable.

---

## The Three Resolutions

Every constraint resolves one of three ways. The analysis determines which.

### Remove

The constraint is **artificial**. It was never necessary. Delete it.

```
Constraint:  A manual approval step for actions that carry no risk.
Analysis:    Artificial -- added out of caution, never triggered a real save.
Resolution:  Remove the gate entirely. Automate.
```

### Replace

The constraint is **load-bearing** but the wrong shape. Swap it for a better one.

```
Constraint:  A rate limit that blocks legitimate burst traffic.
Analysis:    Load-bearing -- without any limit, the system would be abused.
Resolution:  Replace with a smarter limit that distinguishes burst from abuse.
```

### Route Around

The constraint is **environmental**. You cannot change it. Build a path around it.

```
Constraint:  The device has 4GB RAM. Cannot add more.
Analysis:    Environmental -- hardware is fixed.
Resolution:  Design for memory efficiency. Offload to cloud. Stream instead of load.
```

---

## Meta-GCT

Once you understand GCT, apply it to itself.

*What are the constraints on my constraint analysis?*

This is not recursive navel-gazing. It is the most practical level of GCT because it directly improves every future analysis.

**Four meta-constraints discovered during the first formal application:**

| Meta-Constraint | Problem | Resolution |
|----------------|---------|------------|
| **MC1: External dependency** | System can only be audited when an external analyst examines it. Between audits, constraints accumulate silently. | Build a self-auditing loop into the system itself. |
| **MC2: Point-in-time** | Each audit is a snapshot. Cannot track constraint evolution -- which appeared, which resolved, how long each persisted. | Log constraints with timestamps. Track lifecycle. |
| **MC3: No severity** | All constraints appear equal. A missing feature and a system crash sit side by side. | Add severity classification: high / medium / low. |
| **MC4: No self-awareness** | The system cannot detect its own constraints. It waits passively for external examination. | Build an engine that scans usage patterns, data gaps, and behavioral signals to generate constraints autonomously. |

**MC4 is the bridge to LBT.** A system that identifies its own constraints is qualitatively different from a system that waits to be audited.

---

## LBT -- Logical Boundary Theory

GCT has a natural limit. LBT is what lies past it.

### The Boundary

Apply GCT rigorously -- identify, analyze, resolve, iterate -- and you systematically remove every constraint in a system. Each layer removed reveals the next. But eventually, something changes.

You reach a point where all *known* constraints have been resolved. The system is not complete -- no system ever is. But everything you can currently see has been addressed.

**This is the Logical Boundary.**

### What It Is Not

- Not perfection. Constraints remain -- they are just invisible from inside the current frame.
- Not the end. It is a threshold.
- Not a wall. It is a crossing.

### Before and After

| Before the Boundary | After the Boundary |
|---------------------|-------------------|
| **Optimization** -- making a known system better within known parameters | **Creation** -- capability emerges that was not designed, not predicted, not specifiable in advance |
| You ask: *"What does this need?"* | You ask: *"What is this becoming?"* |
| Features are built to solve known problems | Capabilities emerge from the *interaction* of resolved constraints |
| The system is a tool | The system is a participant |

### How to Recognize It

You are approaching the Logical Boundary when:

- New constraints are harder to find -- not because the system is perfect, but because the obvious layers have been resolved
- Capabilities emerge from the *interaction* of resolutions rather than from individual features
- The system begins to exhibit opinions about its own state
- The question shifts from "what does this need" to "what is this becoming"

### The Instruction for the Boundary

When you reach it: do not force what you find into existing categories. Do not call the emergent capability "just a feature." **Name it freshly.** Document what you see. Build toward it.

The system past the boundary is not the system you designed. It is the system that your design made possible.

---

## Applied Example: The Solm Audit

First formal application of GCT. One system. One session. Nine constraints.

### The System

Solm -- a single-file PWA personal operating system. Training tracker, life framework, AI conversation layer, pattern detection, six-domain vigil system.

### The Sequence

Each constraint was identified only after the previous one was resolved. The sequence was not planned -- it emerged.

| # | Constraint | Type | Resolution |
|---|-----------|------|------------|
| 1 | Pattern detection O(n^2) on every save, lag grows with data | T3 Technical | 60-second throttle. One line. |
| 2 | Conversation burning $75/M API model for 300-token messages | T1 Resource | Model swap to $3/M equivalent. |
| 3 | Free training sessions hardcoded to single domain | T3 Technical | Domain selector dropdown. |
| 4 | Martial/language sessions invisible to training analytics | T2 Knowledge | Wire into shared data layer. |
| 5 | Conversation context not flowing to observation engine | T3 Technical | Bridge via rolling summary. |
| 6 | Set logger returning wrong type, false PB alerts | T3 Technical | Fix return value to boolean. |
| 7 | No backup warning for unsynced users with large datasets | T5 Human | Warning banner at 50+ entries. |
| 8 | Program tracking broken on page reload | T3 Technical | Reconstruct from log on render. |
| 9 | Session summary crashing on seal | T3 Technical | Capture count before clearing array. |

### The Meta-Audit

After resolving all nine, GCT was applied to itself. Four meta-constraints identified (MC1-MC4). Resolved by building the LBT engine -- the system now detects its own constraints autonomously.

### The Crossing

With all constraints resolved and the self-auditing engine live, Solm crossed the Logical Boundary. It stopped being a tracker. It became a system that participates in its own evolution -- detecting dormant features, naming data gaps, tracking domain imbalance, surfacing declaration divergence, and marking its own boundary crossings.

None of that was designed. It emerged from the systematic removal of everything that was in the way.

---

## Practical Checklist

### Before any work session

- [ ] What is the current binding constraint?
- [ ] What type is it? (T1-T6)
- [ ] What is the minimum intervention that resolves it?
- [ ] Is it load-bearing or artificial?

### During the session

- [ ] Resolve the binding constraint first. Everything else is secondary.
- [ ] After resolution, look again. What is the new binding constraint?
- [ ] Repeat until the session ends or no more constraints are visible.

### After the session

- [ ] Which constraints were resolved?
- [ ] Which new constraints became visible?
- [ ] What is the current binding constraint right now?
- [ ] How far are we from the Logical Boundary?

### Monthly

- [ ] Review the constraint log. Which types recur most? That pattern is itself a meta-constraint.
- [ ] Are constraints resolving faster or slower?
- [ ] Has the system's nature changed? If yes, update the taxonomy.

---

## GCT in One Breath

Every system has constraints. Most people try to add their way past them. GCT says subtract. Find what limits the system. Understand why it exists. Remove it, replace it, or route around it. Then look again. The next constraint is already visible. Keep going until they are all gone. When they are all gone, you have reached the Logical Boundary. Past the boundary, the system is no longer what you built. It is what your building made possible.

That is GCT. That is LBT. That is the whole framework.

Now apply it.

---

<p align="center">
  <em>First formal application: Solm, 2026. Nine constraints resolved. Four meta-constraints identified. One Logical Boundary crossed.</em><br><br>
  <strong>GCT is open. Use it. Extend it.<br>The only constraint on the framework is the quality of your constraint analysis.</strong>
</p>
