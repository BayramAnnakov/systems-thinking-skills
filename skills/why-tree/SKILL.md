---
name: why-tree
description: Diagnose a hard problem by building an evidence-graded "Why Tree" - a Goldratt Current-Reality Tree that fans many AI agents across the problem's branch-space, grades every node by the kind of evidence behind it, refutes the weak branches, and converges on the ONE system constraint plus the cheapest test that would prove it. Use when someone asks "why is X happening / why are we falling short of a goal", wants a root-cause analysis with real rigor (not a 5-minute 5-Whys), needs to find the real bottleneck before spending build effort, or asks to "stress-test our diagnosis" / "build a current-reality tree". Bilingual triggers: "почему мы не достигаем цели", "построй дерево текущей реальности", "найди настоящее узкое место". Pairs with /constraint-finder, which returns the 5 Focusing Steps once the constraint is located. Token-heavy by design (multi-agent) - it warns and asks for a depth before launching, and only earns its cost when one careful pass could confidently pick the WRONG cause.
argument-hint: [the problem as a gap-vs-goal + any data/files the agents may read, or "interview me"]
---

# Why Tree

A **Why Tree** is 5-Whys grown up. Naive root-causing follows a single thread, stops at the first plausible cause, and cites nothing. A Why Tree (Goldratt's Current-Reality Tree) does the opposite: it **searches the whole branch-space in parallel**, **grades every node by the evidence actually behind it**, **tries to refute its own load-bearing branches**, and **converges on the ONE constraint** that, if removed, unblocks everything else - plus the negative branches (fixes that backfire) and the single cheapest test that would fork-decide the conclusion.

The output is a **self-contained interactive HTML tree** + a one-page **decision doc** (constraint located / do this / don't do that / cheapest test).

This rigor costs tokens - the deep pass runs ~30-55 agents. The skill is **transparent about that cost and gates on a depth choice before launching** (Phase 0). Do not skip the gate.

## What makes it different from a normal root-cause analysis (read first)

1. **Branch-space first, depth second.** Map *all* the candidate "why"s from many independent lenses before deepening any one. A single-thread analysis finds the first cause; a tree finds the real one.
2. **Every node is graded.** Each node carries an **answer-kind** (is this MEASURED, an INSTANCE, EXTERNAL, a CLAIM, an INFERENCE, a HYPOTHESIS, or just FRAMING?) + a **confidence grade** + a **citation**. Ungraded assertions are banned. See `references/answer-kinds.md`.
3. **Refute before you trust.** Load-bearing branches face an adversarial pass that tries to kill them. Branches that die are marked REFUTED/DOWNGRADED *with the evidence that killed them* - kept visible, not deleted (they're the most useful part).
4. **Converge to ONE.** Goldratt: there is one constraint; everything else is noise. The tree forces convergence to 3-5 roots and names the single system constraint - often a **policy/ownership** problem wearing a tooling costume.
5. **Honest census.** If the decisive nodes are still HYPOTHESIS (unmeasured), the tree says so out loud: "a map of where to look, not a verdict."

If you find yourself shipping five co-equal "root causes," a tidy single-thread chain, or a confident verdict built on unmeasured nodes - you did it wrong.

---

## Phase 0 - Frame the apex + the token-cost gate (do not skip)

**(a) Sharpen the apex.** State the problem as ONE undesirable effect — a **declarative STATEMENT of the bad thing**, ideally a gap-vs-goal: *"Too few X vs the N target by <date>"*, *"Free→paid conversion is below T"*. **Not a question** ("Can we evaluate X?" / "Why is conversion low?") — a question can't have children that read as "…because…", which breaks the whole "why" chain. Keep any question as a title; the apex *node* is the statement. The apex is itself a FRAMING node - it's a hypothesis about what the real problem is, and the tree may reframe it. Push back if the apex is a solution in disguise ("why don't we have feature Y" is a smuggled answer, not a problem). Also reframe a **measurement / "how many" question** ("how many X, how relevant, how many meetings?") into the underlying gap or decision ("can we evaluate ROI of Y - and what's the constraint?"); the counts are *evidence* (Phase 1), not the apex. Don't build a tree on a pure data-pull.

**First — does this problem even need the skill?** A blind head-to-head ablation (3 problems) found that on a **clear-cut** problem a single agent *beat* the full workflow at ~1/50th the cost. The machinery only earns itself when **one mind can confidently go wrong**: cohort/measurement traps, several plausible roots competing, a buildable mechanism hidden under a symptom, or more branches/evidence than one context can hold. **If the answer is likely legible to one careful pass, say so and answer it directly — do not run the workflow.** (This is why Quick mode was removed: for a cheap sketch, a single agent dominates it.)

**(a2) Classify the apex: symptom vs target — it picks the output shape.** Two kinds of gap-vs-goal apex want two different answers, and conflating them is a real failure mode (a target apex run as a pure constraint tree once silently dropped a whole growth lever because converge refuted everything down to one):
- **Symptom apex** — *"X is leaking / under-converting / broken."* The classic CRT: converge to the ONE binding constraint. `apexType:'symptom'` (default).
- **Target apex** — *"hit N by date / grow X→Y"* — a number someone **chose**, reachable by several **additive** paths. Run the same tree **plus** a lever-decomposition: the parallel **levers** to N, each **sized** (volume × conversion → contribution), so the verdict is *"here's the bottleneck AND the arithmetic for whether the levers sum to N"* — not a lone bottleneck. `apexType:'target'`.
- **Say the fit out loud for a target apex:** *"This is a growth-target apex. A constraint tree hands you one bottleneck, not a portfolio that sums to N — I'll run it in lever mode so you also get the sized paths and an honest 'do they sum to the number?'."* Levers are **parallel paths, not rival causes**: one may be deprioritized (with a stated reason) but never refuted away like a wrong cause.

**(b) Pick a depth - and state the cost plainly.** Present these two and **wait for an explicit choice**. Describe scale by agent-count and breadth, never by time:

| Depth | Agents (approx) | What it does | When |
|-------|-----------------|--------------|------|
| **Standard** | ~20-36 | 6-lens branch map + grade every node + one refute pass + converge to one constraint + **drill the constraint branch to bedrock**. | The default for a real, confusable decision. |
| **Deep** | ~30-55 | Adds loop-until-dry discovery, a 3-vote multi-lens refute panel, full CLR audit, cheapest-test design, + the constraint drill. The exhaustive pass. | High-stakes, contested, oversized, expensive-to-get-wrong. |

(Agent count = lenses + deepen + **refute fan-out (the multiplier)** + converge + drill; see the blueprint's cap arithmetic. Older "~12-16 / ~25-30" figures predate the caps + drill step.) **Observed cost: ~50k output tokens per agent** - so Standard ≈ 1.0-1.8M, Deep ≈ 1.5-2.7M. State explicitly, e.g.: *"Deep runs ~30-55 subagents and consumes a large number of tokens (~2M+). Confirm before I launch."* Never launch Deep without confirmation; if running headless/delegated with a pre-granted "depth+cost authorized", proceed without pausing. The hard caps (`MAX_DEEPEN`/`MAX_REFUTE`) bound the worst case — an early uncapped run spawned **171 agents** and the blowup rate-limited and *degraded its own refute pass* — so the caps are a correctness guard, not just cost. If the user gave a token budget (a "+Nk" directive), scale depth to it.

**(c) Confidentiality check.** If the source evidence is a client's private data, ask whether the output may name them. Default to an anonymized tree.

---

## Phase 1 - Inventory the evidence sources

Before launching, enumerate what the research agents may actually read - this determines how much of the tree can be MEASURED vs left as HYPOTHESIS:

- **Web** - Exa (`mcp__exa__*`, `mcp__exa-research__*`) and/or the `firecrawl` skill for competitive/external facts, market data, public docs.
- **Local files / data the user names** - a folder, a CSV/JSON export, prior reports, transcripts, DB query results. Ask for paths.
- **Live data** - if a DB or API is reachable and the user authorizes it, agents can run their own reads (this is what produced the "6/6 sampled accounts had a 2nd seat" refutation in the source case).
- **Prior runs (anti-contamination)** - if a previous Why Tree exists for the SAME problem, you may reuse its **frozen evidence brief** (the Phase-1 facts), but do NOT read its decision-doc / tree / verdict. Reading the prior *conclusion* anchors the lenses and leaks (a converge agent once copied "171 agents" from a prior doc straight into its output). Re-derive from evidence, never from the old answer.

**Compute the hard metrics FIRST.** For data-grounded problems, actually pull/compute the key numbers in Phase 1 (run the query, read the files) and pass them into the workflow as a **frozen evidence brief** with "do NOT re-query". This grounds nodes as MEASURED instead of HYPOTHESIS, and stops N parallel agents from re-hammering the same DB/API. Pull shared/expensive evidence ONCE.

**Measurable-in-hand gate (do not skip).** Before any node is graded HYPOTHESIS or CLAIM, ask: *"would one more query/read over evidence I have ALREADY pulled settle this?"* If yes, it is **not** a HYPOTHESIS — run the check and grade it MEASURED. A node that is cheaply measurable from in-hand data but left unmeasured is a worse failure than a genuinely-external unknown, and they must not be lumped together. **Tag every HYPOTHESIS/CLAIM node with a `gap`:** `in-hand` (compute it now — never ship it ungraded), `external` (needs new data we don't have), or `experiment` (needs a test). Only `external`/`experiment` gaps are legitimate unknowns (they become the Data Request); the workflow's deepen guard **fails loud** on any `in-hand` gap left ungraded.

**Distribution-behind-threshold rule.** Whenever you compute a funnel step as a *threshold* (≥N, hit-the-wall, activated, churned), also pull the **distribution** of the underlying quantity before making any causal claim about why people don't cross it. "83% never hit the 50-seat wall" is silent on whether they died at 5 of 50 or 48 of 50 — opposite diagnoses with opposite fixes. A threshold count may not carry a causal claim the distribution would overturn.

Write a one-line **measurability note**: *"X and Y are queryable; the decisive number Z is not in any source we have - it will stay HYPOTHESIS and become the #1 cheapest-test."* This honesty is the point - it tells the user what the tree can and cannot settle.

**The pre-flight data-gate (ask ONCE, here — never mid-run).** This is the one point where the skill should ask *you* for data. Before launching the (expensive) swarm, list the **3-5 metrics that will actually decide the tree**, mark which are queryable from what you gave me and which are not, and ask a single question: *"these decide it; [A,B] I can compute, [C,D] aren't in any source I have — do you have them / a source, or should they stay hypotheses and become the data request?"* Wait for one answer, then launch. A single cheap round-trip here stops a 2M-token run from going blind on a number you had all along. Skip the gate only when the user pre-authorized "just run it" or the sources are obviously complete. **Interaction lives at the edges — this gate and the Phase-5 Data Request — never inside the swarm:** 50 parallel agents can't pause for a question, so an agent that "wants to ask the user" mid-run must instead tag the node `gap:external` and route it to the Data Request.

**"interview me" mode.** If the user invokes with `interview me` (or names no data/files), run a short structured intake BEFORE Phase 2 — this is the channel for evidence that lives in someone's head, not in any queryable source (the team-knowledge an LLM can't pull). In one pass, ask for: the gap-vs-goal numbers; the 3-5 metrics they know off-hand; any files/DB/dashboards the agents may read; and the one or two unknowns they already suspect are decisive. Capture the answers as the frozen evidence brief, then proceed.

---

## Phase 2 - Run the Workflow (the engine)

Use the **Workflow tool** with the blueprint in `references/workflow-template.md`. The shape (scaled by the chosen depth):

1. **Branch-space fan-out** (`parallel()` barrier) - one agent per **lens**, each blind to the others, each emitting candidate WHY-branches under the apex. Lenses: funnel/mechanics · data-skeptic (audit the numbers) · competitive/external · customer-psychology/segments · economics/incentives · product-eng-constraint. More lenses = wider net.
2. **Dedup + bucket** (plain code, not an agent) - merge overlapping branches; bucket into journey **stages** + a **cross-cutting** group.
3. **Deepen + grade** (`pipeline()` per branch) - recursively ask "why does this happen?" **loop-until-dry** to bedrock (Deep) or one level (Standard); attach **answer-kind + confidence + citation** to every node using `references/answer-kinds.md`. Research agents pull evidence from Phase-1 sources here. **Cap the fan-out** (`MAX_DEEPEN`) — dedup first, rank by load-bearing, then drop the tail (and `log()` what was dropped).
4. **Adversarial refute** (perspective-diverse `parallel()`) - for each load-bearing branch, independent skeptics each try to *refute* it from a distinct angle (data-says-otherwise / mechanism-broken / survivorship / mis-attribution). A branch that ≥half refute is marked **REFUTED** or **DOWNGRADED**, keeping the killing evidence.
5. **Converge** - **group** surviving branches into **3-5 root causes** (group ≠ flatten — keep each branch's `children[]` chain intact under its root); locate **THE single system constraint** (if removed, the most branches unblock). The constraint root **must carry the full apex→bedrock why-chain as nested `children[]`** — a childless constraint root is the flat-funnel bug (the rendered output must be a multi-level *tree*, not a 3-band apex→roots→constraint funnel). A final **"drill constraint"** step re-expands that one branch to bedrock if converge flattened it. Flag whether the constraint is **policy/ownership** vs **tooling**. Apply the Evaporating-Cloud check if it's a goal-conflict (see `references/methodology.md`).
6. **Negative branches + cheapest tests + Data Request** - name fixes that would backfire (NEGATIVE), and the 1-3 reads that would most change the conclusion (rank by: cost to get × how much it moves the verdict). Then compile the **Data Request** — the ranked list of evidence still missing (the `external`/`experiment` gaps, never the `in-hand` ones), each tied to the node(s) it would upgrade HYPOTHESIS→MEASURED and ranked by verdict-impact ÷ cost; #1 is whatever would locate the constraint. This is the "what I need from you to turn this map into a verdict" deliverable (the generalized version of: *"I don't have the ROI data — but here are the exact 4 numbers that would answer it"*).
6b. **Lever decomposition + sizing — TARGET APEX ONLY.** When the apex is "hit N", decompose the **additive levers** (the distinct paths to N — acquire-net-new / migrate-the-base / raise-conversion-of-segment-Y), **size** each (volume × conversion → contribution toward N, MEASURED where the data supports it else gap-tagged), tag **Own/Win/Buy + effort**, map the located **constraint** to the lever it binds, and compute **`sizing`** — do the levers credibly **sum to N**, or does the number lean on one unproven lever? **Levers are parallel paths, not rival causes:** a weak lever is `role:'dropped'` **with a `droppedReason`** — never silently cut. (This is the fix for the run that lost a whole lever to "converge to ONE".)
6c. **Narrate (both modes).** A final cheap agent turns the finished tree into a short **`narrative[]`** memo (3-7 `{heading, body}` prose sections: the problem → what the evidence forces → what we know → the constraint / the lever portfolio → do / don't → the honest risk). The HTML renders it at the **top**, so the single artifact self-explains before anyone drills the tree.
7. **Emit structured tree JSON** - the contract for the visualization (strict schema in `references/workflow-template.md`): `apex` (a SHORT undesirable-effect **STATEMENT**, ≤140 chars — NOT a question, NOT the answer), `verdict` (the one-line answer), `apexType` (`symptom`|`target`), `stages[].nodes[]` (with `id/text/kind/grade/role/cite/gap/children`), `roots[]` (**each with a nested `children[]` causal chain — REQUIRED and non-empty on the constraint root**), `constraint`, `negatives[]`, `tests[]`, `dataRequest[]`, `census`, `narrative[]`, and (target apex) `levers[]` + `sizing`. Tag any test/recommendation that rests on an un-refuted CLAIM/HYPOTHESIS node with "(verify first)".

**Phrasing (this is what makes the tree readable as a "why" chain):** every node — `apex`, every `root[].title`, every node `text` — is a **declarative problem/cause STATEMENT** that reads as an answer to "why" of its parent. *"Ad spend is never captured"*, not the topic label *"Cost denominator"*; *"We can't attribute DMs to ads"*, not the bucket *"DM stage"*. **Stages are an analysis scaffold for grouping evidence**, not causal claims — the rendered funnel is **problem → root causes → constraint** and stage names become only context tags, so never rely on a stage label to carry causal meaning. **Never make a "stage" for the apex / verdict / framing itself** — that content IS the apex statement + verdict, not a cause bucket (a stage titled "Apex" or "framing" is a generation bug). Keep `role` within the controlled vocabulary; don't invent roles like "apex-answer".

Save the JSON to disk (e.g. `why-tree-output.json`) before visualizing, so a failed render never loses the analysis.

---

## Phase 3 - Visualize (invoke `frontend-design`)

Render the tree JSON into a **single self-contained HTML file** using `assets/tree-template.html` as the scaffold. **Invoke the `frontend-design` skill** for the aesthetic pass - this is the user's explicit ask: the visualization must be distinctly designed, not the GitHub-default look of the original artifact. (frontend-design is an external Anthropic-provided skill; if it is not installed, `assets/tree-template.html` still renders standalone and the aesthetic pass is optional.)

Requirements the visualization must hit:
- **The narrative memo rendered FIRST** - `narrative[]` as a styled "Read this first" block at the top (above the tree), open by default, so the artifact self-explains in prose before anyone touches the interactive tree. This is the comprehension layer.
- **A prominent one-line `verdict`** rendered under the apex (the answer; the apex states the problem).
- **The lever portfolio (target apex only)** - render `levers[]` as a sized panel (each lever's contribution, Own/Win/Buy, effort; dropped levers shown greyed *with their reason*, never hidden) plus the `sizing` bottom line ("do the levers sum to N?"). Hide the panel when `levers` is absent (a symptom apex).
- **Collapsible, evidence-rich nodes** that stay readable at 25+ nodes (indented disclosure tree, expand/collapse-all).
- **Per-node chips**: answer-kind (color-coded), confidence grade, role badge (ROOT / CONSTRAINT / LEVER / NEGATIVE / REFUTED / DOWNGRADED), and the citation on expand.
- **A constraint "spotlight"** - the single system constraint is visually unmistakable.
- **A convergence map** - a compact branches -> roots -> ONE-constraint diagram (SVG or styled flow), so the "everything funnels to one thing" claim is *seen*, not just asserted.
- **The constraint branch must render as a multi-LEVEL chain, not a single box.** The constraint root carries a nested `children[]` why-chain (apex → because → because → … → bedrock); the spine must show those levels expanded/drillable **in the funnel itself**, not hide them in a side rail. A constraint box with no visible chain beneath it reproduces the "1-level-deep" bug — the whole point is that the eye can follow the chain down to bedrock.
- **Panels**: root causes, negative branches, cheapest tests, the **Data Request** ("what I need from you to turn this map into a verdict" — the ranked missing-evidence list; hide the panel if empty), and the **evidence census** ("~X% MEASURED, but the decisive Y% is HYPOTHESIS"). **Normalize the census to sum to 1.0** before drawing the bar (a 0.92 sum leaves a cosmetic empty sliver).
- Self-contained, opens on double-click, survives being emailed. A `<!-- TWEAK ZONE -->` for last-mile label edits. (To screenshot during dev, serve via `python3 -m http.server` — chrome `navigate` mangles a raw `file://` path.)

Keep `assets/tree-template.html` as the reusable scaffold (data swapped via an inline JSON block); write each run to its own output file.

---

## Phase 4 - Verify (the CLR audit, non-negotiable)

Before showing anyone, audit the tree (Goldratt's "Categories of Legitimate Reservation"):
- **Contradiction check** - does any branch contradict another? Resolve or surface the tension explicitly.
- **One-constraint check** - is exactly ONE system constraint named? If five things are all "critical," convergence failed - send it back.
- **Causality vs correlation** - for each MEASURED->cause arrow, is the cause sufficient, or is there a hidden third factor? Mark INFERENCE honestly.
- **Evidence-honesty census** - compute the answer-kind mix. If the constraint-locating nodes are HYPOTHESIS, the headline must be "where to look," and the cheapest-test must target exactly those nodes.
- **Refutation survived** - every load-bearing branch must show it faced (and survived) the adversarial pass.
- **Multi-level check** - the constraint root must carry a non-empty nested `children[]` chain down to bedrock. A childless constraint root is the flat-funnel bug — send it back to the drill step.
- **Measurable-in-hand check (fail loud).** Every HYPOTHESIS/CLAIM node must carry a `gap` tag (`in-hand` / `external` / `experiment`). An `in-hand` node is a hard error — one query over data already pulled settles it, so the workflow's deepen guard warns loudly and the node must be computed and re-graded MEASURED before shipping. Only `external`/`experiment` gaps are legitimate, and they become the Data Request — never ship an `in-hand` gap as a "cheapest test" or a data-request line.
- **Data-request completeness** - every node still graded HYPOTHESIS/CLAIM with an `external`/`experiment` gap should appear in `dataRequest[]` (or be explicitly out-of-scope). The #1 item must be whatever would locate the constraint.

---

## Phase 5 - Output

Deliver two things:
1. **The interactive tree** (the HTML).
2. **A one-page decision doc** (markdown + matching HTML if the user reviews in HTML per their convention): *Constraint located -> What to do -> What NOT to do (the negative branches) -> The single cheapest test that would fork-decide -> What I need from you (the Data Request: the ranked missing evidence that would upgrade the map to a verdict).* The tree is the diagnosis; the doc is the "so what." **For a target apex**, lead the doc with the **lever portfolio** — a *lever × contribution × Own/Win/Buy × effort* table + the `sizing` "do they sum to N?" line — *then* the binding constraint; the reader needs the paths-to-the-number before the bottleneck inside them.

State the headline honestly: if the system constraint is located but the funnel/mechanism constraint is not, say both - "the policy constraint is settled and needs no more data; the mechanism constraint is unlocated and one query would pin it."

---

## Guardrails

1. **Token honesty first** - never launch Deep without an explicit cost warning and confirmation. The gate is the skill's conscience.
2. **One constraint, not a list** - refuse to ship five co-equal roots. Force convergence to one.
3. **Grade every node** - no ungraded assertions; every HYPOTHESIS must name the test that would settle it.
4. **Refute before trust** - load-bearing branches must survive an adversarial pass, and the dead branches stay visible.
5. **Honest census** - when the decisive nodes are unmeasured, say "map, not verdict." Confident verdicts on HYPOTHESIS nodes are the worst output.
6. **Don't smuggle the answer into the apex** - a problem framed as a missing solution biases the whole tree.
7. **Interaction at the edges, fail loud in the middle** - ask the user only at the Phase-1 pre-flight gate (and the "interview me" intake); never mid-swarm. During the run, never invent and never punt: an `in-hand` gap must be computed (the deepen guard fails loud if not), and only `external`/`experiment` gaps become the Data Request — the honest "here's exactly what I'd need from you" output.
8. **Levers are parallel paths, not rival causes** - on a target apex, the "converge to ONE" discipline applies to root *causes*, not to growth *levers*. Never refute a lever away; a weak lever is `dropped` with a stated `droppedReason` and stays visible. A target apex shipped as a lone bottleneck with no sized portfolio is the wrong output shape.

## Anti-patterns

- A single-thread 5-Whys chain with a tidy bottom (no branching, no refutation = not a tree).
- Nodes asserted without an answer-kind or citation.
- "Five key root causes" with no convergence to one constraint.
- A confident recommendation resting on HYPOTHESIS-graded nodes, with no census disclosing it.
- Deleting refuted branches instead of keeping them with their killing evidence.
- Launching the deep workflow without telling the user it's token-heavy.
- **Pausing the swarm to ask the user** - mid-run questions can't be answered; tag the node `gap:external` and route it to the Data Request instead.
- **Shipping an `in-hand` gap as a "cheapest test" or data-request line** - if one query over data you already pulled settles it, compute it; don't outsource your own homework to the reader.
- **Running the full workflow on a clear-cut problem a single agent would nail** - the ablation shows that's wasted tokens; answer it directly and skip the gate.
- **A flat apex → roots → constraint funnel with childless roots** - the constraint branch must be a visible multi-level chain to bedrock, or it's not a tree.
- An uncapped fan-out (no `MAX_DEEPEN`/`MAX_REFUTE`) - it balloons agent count and rate-limit-degrades the refute pass.
- A GitHub-default-looking tree - the visualization is a deliverable; run it through `frontend-design`.
- **Running a growth-TARGET apex as a pure constraint tree** - it hands back one bottleneck and silently drops the parallel levers; a "hit N" apex needs the sized lever portfolio (+ "do they sum to N?"), not just the binding step.
- **A tree with no narrative** - the HTML must open with the `narrative[]` memo; a raw interactive tree with no prose entry point fails the comprehension test.

## References & assets

- `references/methodology.md` - Goldratt CRT, the WHY-branch logic, CLR audit, constraint location, Evaporating Cloud, negative branches.
- `references/answer-kinds.md` - the evidence taxonomy + confidence grading rubric + citation rules (the core of the rigor).
- `references/workflow-template.md` - the copy-paste `Workflow()` blueprint and the tree-JSON schema.
- `assets/tree-template.html` - the reusable interactive-visualization scaffold.
- `assets/sample-tree.html` - a non-confidential worked example.
