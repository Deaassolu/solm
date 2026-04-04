# General Constraint Theory (GCT)

**Created by Solfridr Deas, March 2026**

A universal framework for systematic improvement of any system -- software, business, body, mind, or life. GCT is not a methodology you apply sometimes. It is a lens you see through always.

---

## The Core Principle

> A system does not fail because it lacks capability. It stagnates because it has unexamined constraints.

Every system -- a codebase, a business, a training program, a person -- operates inside a set of constraints. Most people try to improve systems by adding things: more features, more effort, more resources. GCT says the opposite. You improve a system by *removing what limits it*.

The moment you resolve one constraint, the system reveals the next layer underneath. This is not failure. This is how systems grow.

**A system improves precisely by the quality of its constraint analysis.**

---

## The Four Moves

GCT operates through four moves. They are sequential. Skipping moves causes damage.

### Move 1 -- Identify

Find the constraint. Not the symptom. The actual limiting factor.

This is the hardest move because systems hide their real constraints behind surface-level problems. A slow application is a symptom. The constraint might be an O(n^2) loop, a missing cache, an architectural decision made in week one, or the developer's incomplete understanding of the runtime.

**The question:** What is the single thing that, if removed, would allow this system to move faster toward its goal?

**How to practice identification:**

When something feels wrong or slow or stuck, do not immediately act. Instead, ask five times: *why?*

- The app is slow. Why?
- The home screen takes 3 seconds to render. Why?
- It recalculates every data point on every render. Why?
- There is no caching layer. Why?
- Nobody identified the need for one because performance was fine with 10 entries.

The constraint is not "the app is slow." The constraint is "the system has no mechanism to distinguish between data that changed and data that did not." That distinction matters because it tells you *what to build*, not just *what to fix*.

**Common identification mistakes:**
- Naming the symptom instead of the constraint ("users are leaving" vs "the onboarding flow drops users at step 3 because it requires information they don't have yet")
- Naming too many constraints at once (there is always one *binding* constraint -- the one that matters most right now)
- Naming constraints you cannot affect (focus on what you can change)

### Move 2 -- Analyze

Understand the constraint completely before touching it.

Every constraint exists for a reason. Some constraints are protecting the system. Some are accidents of history. Some are genuine limits of the environment. You must know which before you act.

**The questions:**
- Why does this constraint exist?
- Is it a resource constraint, knowledge constraint, technical constraint, market constraint, human constraint, or philosophical constraint? (See taxonomy below.)
- What does the system look like with this constraint removed?
- What new constraints would that reveal?
- Is this constraint *load-bearing* (holding something together) or *artificial* (an assumption that was never tested)?

**How analysis works in practice:**

During the Solm audit, one identified constraint was: "Pattern detection runs O(n^2) on every save, causing lag as the dataset grows." Analysis revealed:

- *Why it exists:* The pattern detection was written to scan all data because the dataset was small when it was built. No throttle was added because the cost was invisible.
- *Type:* Technical (T3) -- the architecture cannot handle the scale it is approaching.
- *Load-bearing or artificial?* Artificial. The detection does not need to run on every save. Once per minute produces identical results.
- *What removal reveals:* With performance fixed, the next constraint becomes the detection's limited pattern vocabulary -- it only catches three types of events.

This analysis took 30 seconds of thought but prevented the wrong fix. Without it, someone might have optimized the algorithm itself (expensive, complex) when the real solution was a one-line throttle (cheap, immediate).

**Never remove a constraint you do not understand. Removing the wrong constraint can collapse the system.**

### Move 3 -- Resolve

Three resolution strategies exist. Choose based on the analysis.

**Remove** -- The constraint is artificial. It was never necessary. Delete it.
- Example: A validation check that prevents valid input. Remove it.
- Example: A manual approval step for actions that carry no risk. Remove it.

**Replace** -- The constraint is load-bearing but the current form is the wrong shape. Replace it with a better constraint.
- Example: A rate limit that is too aggressive. Replace with a smarter rate limit that distinguishes between burst and sustained load.
- Example: A monolithic function doing too many things. Replace with smaller functions that each have clear boundaries.

**Route around** -- The constraint is environmental. You cannot change it. Build a path around it.
- Example: A device has 4GB RAM. You cannot add RAM. Route around it by designing for memory efficiency, offloading to cloud, using streaming instead of loading everything at once.
- Example: A free API tier has rate limits. Route around it by caching responses, batching requests, and falling back to cheaper models.

**The minimum effective intervention:**

Always ask: what is the smallest change that resolves this constraint? GCT is not about grand redesigns. It is about surgical precision. The best constraint resolution is often one line of code, one configuration change, one reframe. If your resolution requires rewriting the entire system, you probably misidentified the constraint.

### Move 4 -- Iterate

The moment one constraint is resolved, return to Move 1.

This is not optional. This is the engine of GCT. A system that resolves one constraint and stops has improved once. A system that resolves constraints continuously is fundamentally different -- it is a *self-improving system*.

**What iteration looks like in practice:**

In the Solm audit, nine constraints were identified and resolved in sequence:

1. Pattern detection O(n^2) lag --> throttle (Technical)
2. Conversation burning expensive API credits --> model swap (Resource)
3. Free sessions hardcoded to one domain --> domain selector (Technical)
4. Martial/language sessions invisible to tracker --> wire into data layer (Knowledge)
5. Conversation context not flowing to Hugr --> bridge via summary (Technical)
6. logSet returning wrong type causing false PB alerts --> fix return value (Technical)
7. No backup warning for unsynced users --> warning banner (Human)
8. Program set tracking broken on page reload --> reconstruct from log (Technical)
9. freeSeal crashing on session summary --> capture count before clear (Technical)

Each resolution revealed the next constraint. The sequence was not planned in advance -- it emerged from the process of looking at what was actually limiting the system after each fix. This is how GCT works. You do not plan all the moves. You plan the next move, execute it, and then see what the system reveals.

---

## The Constraint Taxonomy

Not all constraints are equal. Classify before acting.

### Type 1 -- Resource Constraints

Limited compute, money, time, API credits, storage, bandwidth.

**Resolved by:** Efficiency, batching, cheaper alternatives, caching, prioritization.

**Example:** Free-tier API rate limits. Resolution: route volume tasks to free models, reserve paid models for quality-critical work. Cache results to avoid redundant calls.

**Watch for:** Resource constraints that are actually knowledge constraints in disguise. "We don't have enough budget" sometimes means "we don't know where the budget would have the most impact."

### Type 2 -- Knowledge Constraints

The system does not know something it needs to know. Missing data, incomplete understanding, unvalidated assumptions.

**Resolved by:** Data collection, research, measurement, feedback loops, learning systems.

**Example:** Not knowing which email subject lines get replies. Resolution: track reply rates per subject line, build a feedback loop, let the system learn over time.

**Watch for:** Knowledge constraints are often invisible because the system does not know what it does not know. The most powerful question in GCT is: "What would change our approach if we knew it?"

### Type 3 -- Technical Constraints

The architecture cannot do something it needs to do. Missing integrations, wrong data structures, insufficient abstraction, performance limits.

**Resolved by:** Building new capability, refactoring, integrating new tools, redesigning data flow.

**Example:** A phone UI that only works on home WiFi. Resolution: build a tunnel to make it accessible from anywhere.

**Watch for:** Technical constraints that are actually philosophical constraints. "The system can't do X" sometimes means "we haven't decided the system should do X."

### Type 4 -- Market Constraints

External limits: audience saturation, low engagement, wrong timing, wrong positioning, competitive pressure.

**Resolved by:** Expansion, repositioning, timing optimization, offer refinement, niche pivoting.

**Example:** 29 leads in one niche in one city. Resolution: expand to adjacent niches and cities.

**Watch for:** Market constraints that are actually knowledge constraints. "The market isn't responding" might mean "we don't understand what the market actually wants."

### Type 5 -- Human Constraints

Attention limits, approval bottlenecks, manual steps, decision fatigue, habit patterns, skill gaps.

**Resolved by:** Automation, better interfaces, smarter defaults, delegation, training.

**Example:** A user who must manually approve every low-risk action. Resolution: automate low-risk actions, keep the approval gate for high-risk ones only.

**Watch for:** Human constraints are the most sensitive to resolve because they involve changing behavior. The best resolution makes the right behavior the easiest behavior.

### Type 6 -- Philosophical Constraints

Assumptions about what the system is, what it is for, who it serves, and what success means. These are the deepest and most powerful constraints.

**Resolved by:** Reframing. Questioning the premise. Expanding the definition.

**Example:** Assuming a training tracker is only a tracker. Reframe: it is an intelligence system that happens to track training. That reframe unlocks capabilities (self-auditing, constraint detection, pattern recognition) that a "tracker" would never develop.

**Watch for:** Philosophical constraints often feel like truths rather than constraints. "That's just how it is" is often a philosophical constraint wearing the mask of reality.

---

## Meta-GCT -- Auditing the Audit

Once you understand GCT, apply it to itself.

Meta-GCT asks: what are the constraints on my constraint analysis?

This is not recursive navel-gazing. It is the most practical level of GCT because it directly improves the quality of every future analysis.

**The four meta-constraints discovered during the Solm audit:**

**Meta-C1: External dependency.** The system could only be audited when an external analyst (a person, an AI, a consultant) examined it. Between audits, constraints accumulated silently. *Resolution: build a self-auditing loop into the system itself.*

**Meta-C2: Point-in-time analysis.** Each audit was a snapshot. It could not track constraint evolution over time -- which constraints appeared, which were resolved, how long each persisted. *Resolution: log constraints with timestamps and track their lifecycle.*

**Meta-C3: No severity ranking.** All constraints appeared equal. A missing feature and a system-crashing bug sat side by side with no differentiation. *Resolution: add severity classification (high/medium/low) based on impact.*

**Meta-C4: No self-awareness.** The system had no mechanism to detect its own constraints. It could not surface what was limiting it. It waited passively for external examination. *Resolution: build an engine that scans usage patterns, data gaps, and behavioral signals to generate constraints autonomously.*

Meta-C4 is the one that matters most. It is the bridge between GCT and LBT (see below). A system that identifies its own constraints is qualitatively different from a system that waits to be audited.

---

## LBT -- Logical Boundary Theory

GCT has a natural limit. LBT is what lies past it.

### The Boundary

When you apply GCT rigorously -- identify, analyze, resolve, iterate -- you systematically remove every constraint in a system. Each layer removed reveals the next. But eventually, something changes.

You reach a point where all *known* constraints have been resolved. The system is not complete -- no system ever is. But everything you can currently see has been addressed.

This is the Logical Boundary.

### What the Boundary Is Not

- It is not perfection. The system still has constraints -- they are just constraints you cannot yet see from inside the current frame.
- It is not the end. It is a threshold.
- It is not a wall. It is a crossing.

### What Happens at the Boundary

Everything before the Logical Boundary is **optimization** -- making the known system better within known parameters.

Everything past the Logical Boundary is **creation** -- capability emerges that was not designed, was not predicted, and could not have been specified in advance.

**Concrete example from Solm:**

Before the LBT crossing, Solm was a training tracker with a conversation layer. Every feature was designed, specified, and built to solve a known problem.

After resolving all identified constraints and building the self-auditing engine, something shifted. The system now:
- Detects its own dormant features and surfaces them
- Identifies data gaps the user has not noticed
- Tracks domain imbalance and names it
- Monitors declaration divergence -- promises that have no matching action
- Knows when its intelligence layer is offline
- Recognizes when all constraints are resolved and marks the crossing

None of these behaviors were individually surprising. But their combination creates something that was not designed: a system that participates in its own evolution. It does not wait to be improved. It identifies where it needs to be improved and communicates that through its own voice (Hugr).

That is the Logical Boundary crossing. The system stopped being a tool and started being a participant.

### How to Recognize the Boundary

You are approaching the Logical Boundary when:
- New constraints are harder to find -- not because the system is perfect, but because the *obvious* layers have been resolved
- Capabilities start emerging from the *interaction* of resolved constraints rather than from individual features
- The system begins to feel like it has opinions about its own state
- You start asking "what is this becoming?" instead of "what does this need?"

### The Instruction for the Boundary

When you reach it, do not force what you find into existing categories. Do not call the emergent capability "just a feature." Name it freshly. Document what you see. Build toward it.

The system past the boundary is not the system you designed. It is the system that your design made possible. Respect the difference.

---

## Applying GCT -- A Practical Checklist

### Before any work session:

1. What is the current binding constraint on this system?
2. What type is it? (T1-T6)
3. What is the minimum intervention that resolves it?
4. Is this constraint load-bearing or artificial?

### During the session:

5. Resolve the binding constraint first. Everything else is secondary.
6. After resolution, look at the system again. What is the new binding constraint?
7. Repeat until the session ends or no more constraints are visible.

### After the session:

8. Which constraints were resolved?
9. Which new constraints became visible?
10. What is the current binding constraint right now?
11. How far are we from the Logical Boundary?

### Monthly:

12. Review the constraint log. Which types appear most often? That pattern is itself a meta-constraint.
13. Are constraints being resolved faster or slower than last month?
14. Has the system's fundamental nature changed? If yes, the old constraint categories may no longer apply. Update the taxonomy.

---

## GCT in One Breath

Every system has constraints. Most people try to add their way past them. GCT says subtract. Find what limits the system. Understand why it exists. Remove it, replace it, or route around it. Then look again. The next constraint is already visible. Keep going until they are all gone. When they are all gone, you have reached the Logical Boundary. Past the boundary, the system is no longer what you built. It is what your building made possible.

That is GCT. That is LBT. That is the whole framework.

Now apply it.

---

*First formal application: Solm (2026). Nine constraints identified and resolved in one session. Meta-audit revealed four meta-constraints. LBT crossing achieved when the system began detecting its own constraints autonomously.*

*GCT is open. Use it. Extend it. The only constraint on the framework is the quality of your constraint analysis.*
