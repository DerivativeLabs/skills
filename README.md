# Derivative Labs Skills

Claude Code skills for marketing, content, and decision-making. Built by [Derivative Labs](https://derivative.io) — used daily across our portfolio of 20+ AI agents.

## Install

```bash
# Install all skills
npx skills add derivativelabs/skills

# Or pick specific ones
npx skills add derivativelabs/skills --skill adversarial-reviewer
npx skills add derivativelabs/skills --skill brand-artifact
npx skills add derivativelabs/skills --skill content-angle
```

Works across 38+ agents: Claude Code, Codex, Cursor, Gemini CLI, GitHub Copilot, and more.

## Skills

### Decision Quality & Risk Management

| Skill | What It Does | Invoke |
|-------|-------------|--------|
| **adversarial-reviewer** | Challenge proposed actions from a robustness perspective. Not "is the code correct" but "should we be doing this at all?" | `/adversarial-reviewer` |
| **decision-review** | Audit decisions for judgment quality, compliance bias, and manipulation vulnerability. | `/decision-review` |

### Content & Brand

| Skill | What It Does | Invoke |
|-------|-------------|--------|
| **brand-artifact** | Generate research-driven brand artifacts — voice guides, community profiles with coach personas, product state docs. | `/brand-artifact` |
| **content-angle** | Pre-writing angle selection using a player-coach dialectic. Generates 3 competing angles, tests against platform personas. | `/content-angle` |
| **authenticity-reviewer** | 7-point authenticity check for community content. Catches AI tells, tone mismatches, and planted specificity. | `/authenticity-reviewer` |

### Knowledge & Analysis

| Skill | What It Does | Invoke |
|-------|-------------|--------|
| **pattern-extractor** | Extract error, success, and decision patterns from coding session transcripts. Scores by frequency × impact. | `/pattern-extractor` |
| **transcript-query** | Search AI coding transcripts across Claude Code, Codex, Cursor, and Gemini CLI with one query. | `/transcript-query` |

## How Skills Work

A skill is a markdown file (`SKILL.md`) in your project's `.claude/skills/` directory. Claude loads it when relevant — or when you type the slash command.

Skills load progressively:
1. **Frontmatter** (~100 words) — always in context
2. **Body** (<500 lines) — loaded when triggered
3. **References** (unlimited) — loaded on demand

This keeps token usage low while giving Claude specialized expertise.

## What These Skills Are Built For

We run marketing for a portfolio of AI products. These skills handle:
- **Brand consistency** across platforms (voice guides, community personas)
- **Content quality gates** (3-reviewer loops, authenticity checks, fact verification)
- **Institutional knowledge** (pattern extraction, transcript search)
- **Decision rigor** (adversarial review, manipulation detection)

Read the full story: [Best Claude Code Skills for Marketers (2026)](https://helloconvo.com/blog/best-claude-code-skills-for-marketers)

## License

MIT
