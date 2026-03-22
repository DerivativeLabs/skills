---
name: content-angle
description: Pre-writing angle selection for text content. Generates 3 competing angles, tests each against the platform coach persona from community-profile.md, and outputs a structured angle-brief.md that constrains the writing step. Runs before any draft is written.
category: content
---

# Content Angle Skill

## Purpose

The single most expensive mistake in content production is writing the wrong thing beautifully.

Content angle selection happens **before** writing. The angle determines:
- What hook opens the piece
- What frame organizes the argument
- What the piece has one job to do
- What evidence must be verified before writing begins

This skill uses a **player-coach dialectic** to select the angle. The player (content strategist) proposes. The coach (community persona from research) decides what actually lands.

---

## When to Invoke

Invoke at the start of any new text content piece, before touching a keyboard.

Do NOT invoke if:
- A draft already exists with a defined angle — use `text-review-loop` instead
- The content ID already has an `angle-brief.md` — review and amend it instead
- This is a revision of existing content — skip directly to `text-review-loop`

---

## Inputs

| Input | Source | Required |
|-------|--------|----------|
| Brand slug | `docs/brand-configs/<slug>.json` | Yes |
| Platform | e.g. `reddit/r/SomeSubreddit`, `blog`, `linkedin`, `twitter` | Yes |
| Content type | `reddit-post`, `blog-post`, `linkedin-post`, `twitter-thread`, `narrative` | Yes |
| Topic prompt | What the piece is about (1-3 sentences) | Yes |
| Content ID | e.g. `BRAND-001` | Yes (assign before starting) |

---

## Process

### Step 1: Load Brand Artifacts

Read all of the following:
- `docs/brand-artifacts/<slug>/voice-guide.md` — tone, anti-patterns, authenticity tax
- `docs/brand-artifacts/<slug>/community-profile.md` — per-platform audience + coach persona
- `docs/brand-artifacts/<slug>/product-state.md` — what's verified and safe to claim
- `docs/brand-configs/<slug>.json` — brand config (competitive context, positioning)

If any artifact is missing, stop and run `bun scripts/brand-artifact.ts init <slug>` first.

---

### Step 2: Extract Coach Persona

From `community-profile.md`, extract the coach persona for the target platform.

The coach persona describes:
- Who they are (role, experience, community standing)
- The question they ask when they open a post
- What makes them upvote
- What makes them scroll past
- What makes them report or downvote

This persona is your adversary throughout the angle loop. They are never satisfied by generic content.

---

### Step 3: Player Generates 3 Angles

For the given topic, generate exactly 3 competing angles.

Each angle must specify:

```
## Angle [A/B/C]: <angle name>

**Hook** (first sentence of the post):
> [exact first sentence — commit to it]

**Frame**: [the organizing idea — e.g. "diagnosis", "builder confession", "before/after", "failure story", "infrastructure argument"]

**One job**: [what this piece does — verb + specific outcome]

**Evidence required**: [what must be verified before writing — specific claims, code examples, API calls]

**Platform fit**: [why this angle works for this specific platform + audience]

**Risks**: [what could go wrong with this angle — authenticity risk, factual risk, timing risk]
```

**Angle diversity requirement**: The 3 angles must be genuinely different in frame, not just different in wording. A diagnostic frame and an announcement frame are different. "Before/after" and "failure story" are different. Three variations of "here's our product" are not different.

---

### Step 4: Coach Evaluates Each Angle

For each angle, the coach persona speaks in first person:

```
**Coach on Angle [A/B/C]:**

[Would I upvote this? Why or why not. Be direct.]

[What triggers my "scroll past" reflex here, if anything?]

[What triggers my "report or downvote" reflex here, if anything?]

[What would make this angle work, if it doesn't already?]

**Verdict**: STRONG | VIABLE | WEAK
```

The coach is allowed to reject all 3 angles. If that happens, the player must generate 3 new angles.

**Maximum 2 generation rounds** (6 angles total). If both rounds are fully rejected, the topic itself needs rethinking — the research is insufficient, or the platform/audience is wrong for this subject. Escalate to the brand owner with a summary of what was rejected and why.

---

### Step 5: Player Responds to Weak/Viable Verdicts

For any angle not marked STRONG:

```
**Player response to Coach on Angle [A/B/C]:**

[Respond specifically to the coach's concerns. Either defend or concede each point.]

[If defending: show the evidence or reasoning that addresses the concern.]

[If conceding: propose an amendment to the angle that addresses the concern.]
```

The player cannot just say "you're right." They must either fix the angle or articulate why the concern doesn't apply.

---

### Step 6: Coach Final Selection

The coach selects one angle as the winner:

```
**Coach Final Selection**: Angle [A/B/C]

**Rationale**: [1–3 sentences: what makes this the right angle for this piece, this platform, this moment]

**Required before writing**:
- [List any evidence the player must verify before a single word of the draft is written]
- [If none required: state "None — facts in this angle are already verified in product-state.md"]

**One constraint**: [The single most important thing the writer must not violate. Usually: the one anti-pattern from voice-guide.md most likely to creep in for this specific angle.]
```

If no angle is clearly stronger, the coach selects the least risky one and states the improvement needed.

---

### Step 7: Write Angle Brief

Save `angle-brief.md` to `docs/content-drafts/<slug>/<content-id>-angle-brief.md`.

**Format:**

```markdown
# Angle Brief — <content-id>

**Brand**: <slug>
**Platform**: <platform>
**Content type**: <type>
**Written**: <ISO timestamp>

---

## Selected Angle

**Hook** (exact first sentence):
> [the agreed hook — writer must use this or improve it, not abandon it]

**Frame**: [frame name]

**One job**: [verb + specific outcome]

---

## Evidence Checklist

Before writing, verify each item is CONFIRMED or has a verified source in product-state.md:

- [ ] [claim 1] — source: [where to verify]
- [ ] [claim 2] — source: [where to verify]
- [ ] [No evidence required — all claims already verified] ← use this if applicable

Writing does NOT begin until all items are checked.

---

## Tone Constraints

From voice-guide.md, the constraints that apply specifically to this angle:

- DO: [specific positive instruction]
- DO: [specific positive instruction]
- DO NOT: [the most likely anti-pattern for this angle]
- DO NOT: [second most likely anti-pattern]

---

## The Authenticity Tax for This Piece

What makes this piece credible rather than promotional:

[1–2 sentences: the specific honesty element this piece must include. Usually: limitation, failure story, setup complexity, or one thing the product doesn't solve yet.]

---

## What the Adversarial Gate Will Check

Pre-fill the adversarial gate conditions that will need to be satisfied:

1. [condition 1 — usually: personal vs brand account decision if Reddit first-person]
2. [condition 2 — usually: an unverified claim or code example]
3. [condition 3 — if applicable]

---

## Rejected Angles

| Angle | Frame | Rejected because |
|-------|-------|------------------|
| [A] | [frame] | [coach's reason] |
| [B] | [frame] | [coach's reason] |

---

## Coach Persona Used

Platform: <platform>
Source: `docs/brand-artifacts/<slug>/community-profile.md`
[First sentence of persona — confirms which persona was used]
```

---

## Output

After the angle brief is saved:

1. Update `docs/content-pipeline.md` — add the piece to the Active Queue with status `draft` and note "angle brief complete"
2. The writer (next session or next step) reads the brief and begins the draft
3. The draft must reference the content ID in its frontmatter

---

## Integration Points

- **Feeds into**: `text-review-loop` (the draft written from this brief goes through the review loop)
- **Reads from**: `brand-artifact` outputs — community-profile.md, voice-guide.md, product-state.md
- **Used by**: content agent before any new text content piece
- **Parallel**: video-preproduction skill (same gate concept — platform decision before production begins)

---

## When the Angle Brief Already Exists

If resuming an in-progress content piece:

1. Read the existing `angle-brief.md`
2. Check the evidence checklist — are all items confirmed?
3. If not confirmed: resolve before writing
4. If confirmed: begin the draft following the brief
5. Do NOT re-run the angle loop — the decision is made

To change the angle after writing has begun, mark the draft as `abandoned`, run a new angle loop, and start fresh. Patching an angle mid-draft produces incoherent pieces.

---

## Related Skills

- `text-review-loop` — reviews the draft written from this brief
- `brand-artifact` — produces the community-profile.md and voice-guide.md this skill reads
- `video-preproduction` — equivalent upstream step for video content
- `adversarial-reviewer` — the gate whose conditions are pre-filled in the angle brief
