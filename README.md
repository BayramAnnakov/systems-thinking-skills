# Systems Thinking — Skills

Reusable [Claude Code](https://claude.ai/claude-code) skills for systems thinking, drawn from the **AI + Systems Thinking** course (Empatika, Season 2026) by [Bayram Annakov](https://github.com/BayramAnnakov). Installable as a Claude Code plugin marketplace.

The course is hands-on: participants draw stock-flow diagrams by hand, run live multiplayer simulations, and use these AI skills as a *critic and amplifier* — never as a designer. The skills enforce that pedagogy: they refuse to draw the diagram for you.

## Available Skills

### `/ai-systems-coach` — Stock-flow diagram critic & archetype identifier

**You draw the diagram. The skill grades it.**

Critiques a hand-drawn stock-flow / causal-loop diagram, identifies the system archetype, and surfaces Meadows leverage points. Built around the three core archetypes covered in Workshop 2: **Limits to Growth**, **Shifting the Burden**, **Fixes that Fail**.

**What it does:**
- Validates your stocks (only true accumulators, not flows in disguise)
- Identifies your loops (R/B) and flags missing delays
- Calls out missing variables that would be visible to a system dynamics modeller
- Names the archetype using a strict 3-step decision tree (StB vs FtF vs LtG) — or refuses if the structure isn't one of the three
- Outputs a Mermaid diagram of the structure (so you can see it on one screen)
- Surfaces 3 leverage points ranked by Meadows hierarchy (parameters → goals → paradigms)
- Prepares a clean stock-flow specification you can hand to Workshop 3 for code-based simulation

**What it refuses to do:**
- Design a diagram from scratch — drawing is the act of thinking, do it on paper first
- Invent variables you didn't name
- Identify an archetype if your input doesn't match one of the three canonical structures (it tells you exactly what's missing)

**Example:**
```
/ai-systems-coach

Stock: Active Customers
Inflow: New signups (driven by referrals from Active Customers)
Outflow: Churn (driven by Service Quality, which drops as Active Customers grows past Capacity)
R: Active Customers → Referrals → New signups → Active Customers
B: Active Customers → Service Quality (delay 2 weeks) → Churn → Active Customers
```

The skill responds bilingually (English and Russian) — it auto-detects the language of your input.

## Installation

### Option 1: Plugin install (recommended)

In Claude Code:
```
/plugin marketplace add BayramAnnakov/systems-thinking-skills
/plugin install ai-systems-coach@systems-thinking-skills
/reload-plugins
```

### Option 2: Clone and copy skills

```bash
git clone https://github.com/BayramAnnakov/systems-thinking-skills.git
cp -r systems-thinking-skills/skills/ai-systems-coach ~/.claude/skills/
```

### Option 3: Single-file via curl

```bash
mkdir -p ~/.claude/skills/ai-systems-coach
curl -o ~/.claude/skills/ai-systems-coach/SKILL.md \
  https://raw.githubusercontent.com/BayramAnnakov/systems-thinking-skills/main/skills/ai-systems-coach/SKILL.md
curl -o ~/.claude/skills/ai-systems-coach/PROMPT.md \
  https://raw.githubusercontent.com/BayramAnnakov/systems-thinking-skills/main/skills/ai-systems-coach/PROMPT.md
curl -o ~/.claude/skills/ai-systems-coach/MERMAID-CHEATSHEET.md \
  https://raw.githubusercontent.com/BayramAnnakov/systems-thinking-skills/main/skills/ai-systems-coach/MERMAID-CHEATSHEET.md
```

## Usage

```
/ai-systems-coach    # then paste your structured diagram
```

Use it after Workshop 2 (where you learn the three archetypes by hand) to grade your homework, and before Workshop 3 (where the diagram becomes runnable code).

## Course context

Part of the **AI + Systems Thinking** course (6 biweekly online sessions, 2026):

1. How Systems Work — stocks, flows, feedback, delays
2. **System Archetypes** — Limits to Growth, Shifting the Burden, Fixes that Fail *(this skill ships with W2)*
3. From Diagram to Simulation — turning your diagram into runnable code
4. Theory of Constraints — finding your bottleneck
5. FishBanks — live competitive market simulation
6. From Understanding to Transformation — meta-framework

More skills will land in this repo as later workshops ship.

## Pedagogical philosophy

These skills enforce a strict separation: **humans draw, AI critiques**. We don't ship AI tools for skills participants haven't first practiced manually. That's why `ai-systems-coach` ships with W2 (where the archetype vocabulary is taught) and not earlier — and why it refuses to draw a diagram from scratch even when asked nicely.

If you want to design from scratch, draw it on paper first. The friction *is* the lesson.

## License

MIT — see [LICENSE](LICENSE).
