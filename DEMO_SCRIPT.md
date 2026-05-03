# Hermes Volta Demo Script: The Learning Loop

This demo shows Hermes Volta improving its own `skills/volta/SKILL.md` after a successful design workflow. The key point: Session 1 exposed a missing behavior, and Session 2 made that behavior part of the skill so future runs start smarter.

## Session 1 -> Session 2 Diff

In Session 1, Volta could design circuits when the user gave a target circuit and cutoff frequency:

```text
User: design a 1 kHz RC low-pass filter at 5 V
Volta: computes R/C, runs simulation, exports plots, PCB, Gerbers, and report.
```

But project-level prompts were underspecified:

```text
User: I'm building a guitar pedal
Old behavior: Volta could ask for fc or component values before designing.
```

In Session 2, Hermes learned the missing workflow and patched `SKILL.md` so project descriptions trigger autonomous recommendations.

```diff
@@ skills/volta/SKILL.md
 Load Volta when the user asks to:
 
 - Design an analog filter from a target cutoff or center frequency.
 - Choose resistor, capacitor, or inductor values.
 - Run a circuit simulation and produce plots.
 - Export a simple KiCad PCB or Gerber package.
 - Produce a JLCPCB-ready BOM search plan.
 - Analyze cutoff error, tolerances, or manufacturable E24 alternatives.
 - Convert a hand-drawn passive filter sketch into a simulated design.
+- Recommend a circuit autonomously from a project description such as a guitar pedal, Arduino audio project, microphone preamp, sensor interface, or power-supply noise cleanup.
 
 ## 4. Procedure
 
+## Autonomous Design Mode
+
+When the user describes a project instead of specifying a circuit, Volta should autonomously recommend and generate the first useful circuit design.
+
+CRITICAL: Autonomous Design Mode is an execution mode, not a suggestion mode. Do not ask "Want me to..." or request confirmation before simulation. Once the project type is understood, decide the circuit, explain the decision briefly, and immediately run the full Faraday pipeline in the same turn.
+
+CRITICAL: Do not fall back to the project default supply voltage until the supply-voltage search has been attempted. If the search result gives a common project supply, use that value and mention it. Use the project default only when search is unavailable or inconclusive.
+
+Trigger this mode when the user describes a project without specifying `fc`. Typical examples:
+
+- "I'm building a guitar pedal"
+- "I'm making an Arduino audio project"
+- "I'm designing a microphone preamp"
+- "I'm building a sensor interface"
+- "I need to clean up my power supply noise"
+
+Trigger keywords include: "building", "making", "designing", "project", "for my", and "I need".
+
+Volta should not ask the user for `fc` or component values. Instead:
+
+1. Run `web_search` for `"{project type} typical signal frequency range"`.
+2. Run `web_search` for `"{project type} supply voltage common"`.
+3. From the search results, determine:
+   - Recommended circuit type
+   - Target `fc`
+   - Supply voltage
+   - Why this design makes sense
+4. Tell the user what Volta decided and why before running the pipeline.
+5. Run the full Faraday pipeline using those autonomous decisions. Do not stop after explaining the recommendation.
+6. In Telegram delivery, include the reasoning:
+
+```python
+send_message(platform="telegram", message="💡 Why this design: {explanation}")
+```
```

## Exact Lines Hermes Added Autonomously

These are the durable workflow lines added to `skills/volta/SKILL.md`:

```text
## Autonomous Design Mode

When the user describes a project instead of specifying a circuit, Volta should autonomously recommend and generate the first useful circuit design.

CRITICAL: Autonomous Design Mode is an execution mode, not a suggestion mode. Do not ask "Want me to..." or request confirmation before simulation. Once the project type is understood, decide the circuit, explain the decision briefly, and immediately run the full Faraday pipeline in the same turn.

CRITICAL: Do not fall back to the project default supply voltage until the supply-voltage search has been attempted. If the search result gives a common project supply, use that value and mention it. Use the project default only when search is unavailable or inconclusive.

Trigger this mode when the user describes a project without specifying `fc`. Typical examples:

- "I'm building a guitar pedal"
- "I'm making an Arduino audio project"
- "I'm designing a microphone preamp"
- "I'm building a sensor interface"
- "I need to clean up my power supply noise"

Trigger keywords include: "building", "making", "designing", "project", "for my", and "I need".

Volta should not ask the user for `fc` or component values. Instead:

1. Run `web_search` for `"{project type} typical signal frequency range"`.
2. Run `web_search` for `"{project type} supply voltage common"`.
3. From the search results, determine:
   - Recommended circuit type
   - Target `fc`
   - Supply voltage
   - Why this design makes sense
4. Tell the user what Volta decided and why before running the pipeline.
5. Run the full Faraday pipeline using those autonomous decisions. Do not stop after explaining the recommendation.
6. In Telegram delivery, include the reasoning:
   "💡 Why this design: {explanation}"
```

## Demo Narration

Session 1 proves Volta can complete a design when the circuit target is explicit.

Session 2 proves the learning loop: once a gap appears, Hermes patches the skill with a reusable rule. The next time someone says, "I'm building a guitar pedal," Volta does not ask for missing filter math. It searches typical signal range and supply voltage, chooses a circuit, explains the engineering reason, runs simulation, exports artifacts, and sends the design package.

That is the loop working:

```text
user behavior -> discovered missing workflow -> SKILL.md patched -> future sessions inherit the improvement
```
