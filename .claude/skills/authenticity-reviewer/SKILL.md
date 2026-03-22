---
name: authenticity-reviewer
description: Fast single-pass adversarial review for community content (Reddit comments, short-form posts, HN threads). Checks AI tells, tone match, value-add vs. existing thread, and account-level risk. Runs before posting, not after a full pipeline. Lighter and faster than text-review-loop.
---

# Authenticity Reviewer

## Purpose

Community content fails differently than brand content. Brand content fails when it's off-voice or factually wrong. Community content fails when it **reads as AI** or **feels planted**. A single inauthentic comment from a warming account can get called out, shadowbanned, or worse — make the account toxic before <content-id> is posted.

This skill is the lightweight gate before posting **any community content** — Reddit comments, HN replies, Indie Hackers threads, Bluesky posts. It takes 2 minutes and prevents mistakes that take 2 weeks to recover from.

**Use this skill, not text-review-loop, for:**
- Reddit comments and posts (especially from warming accounts)
- HN Show/Ask submissions
- Bluesky replies in developer communities
- Any short-form content where you're participating, not broadcasting

**Use text-review-loop for:**
- Planned brand content (blog posts, <content-id> Reddit post, LinkedIn essays)
- Content going through the full approval-gate pipeline
- Anything requiring brand artifact consistency checks

---

## Inputs Required

Before running, gather:

1. **The draft** — the comment or post you're about to submit
2. **Thread context** — title, score, top 5-8 existing comments (with author + score)
3. **Target community** — subreddit, HN, Bluesky, etc.
4. **Account context** — karma, age, posting history (if warming account)

```bash
# Fetch thread context from Reddit JSON API
curl -s "https://www.reddit.com/r/<sub>/comments/<id>/<slug>.json" \
  -H "User-Agent: AuthReviewer/1.0 (research)" | \
  jq '[.[1].data.children[] | select(.kind == "t1") | {author: .data.author, score: .data.score, body: .data.body}] | .[0:8]'
```

---

## The 7-Point Authenticity Check

Work through each point. Flag issues. Issue a verdict at the end.

### 1. Value-Add Check
**Question:** Does this comment add something the existing comments haven't covered?

- Read all top comments first
- Map what's already been said (specific patterns, angles, counter-arguments)
- Identify what the draft contributes that's genuinely new
- **Flag if:** The draft restates what's already there, or makes a point that's clearly lower quality than an existing comment

### 2. AI Tell Scan
**Question:** Does any phrase, structure, or sentence pattern flag as AI-generated?

Common AI tells in community content:
- Opener that validates the post before pivoting ("This is great. The thing I'd add...")
- Three-paragraph essay structure in a comment context
- Smooth transitions between unrelated points ("One thing to watch for...")
- Overly balanced perspective (acknowledging both sides when a real dev would just have an opinion)
- Polished sentence rhythm — real comments have rough edges
- Compound sentences that are too clean: "X → Y. A → B. C → D."
- Hedging language: "might," "could," "tends to" when real devs say "does" or "doesn't"
- Named frameworks without attribution ("The credibility formula," "The half-lives framing")

**Flag any detected tells. They must be rewritten or cut.**

### 3. Specificity Credibility Check
**Question:** Are the specific details (error messages, version numbers, config values, gotchas) genuine — or do they feel inserted to signal credibility?

- Genuine specificity: arises naturally from the experience being described
- Planted specificity: technically accurate but disconnected from what the thread is actually about
- **The test:** Would a developer who had this experience actually mention this detail *in this thread*? Or does it belong in a different thread?

**Flag if:** A specific detail is technically real but contextually forced.

### 4. Tone Match Check
**Question:** Does the comment match how people actually write in this community?

- Check existing comment tone: Are they casual or formal? Do they use code blocks? Do they ask questions or make statements?
- Check sentence length: Short and punchy, or longer and discursive?
- Check vocabulary: Community jargon vs. generic tech terms
- Check opinion directness: Do commenters hedge, or do they state opinions flat?
- **Flag if:** The draft is noticeably more polished, longer, or more structured than the top comments

### 5. Connection Check
**Question:** Does the comment actually engage with the specific post or thread, or is it generic advice that could apply anywhere?

- A real commenter references something specific from the post (a phrase, a design decision, a code snippet)
- Generic comments ("great pattern, I do something similar") feel detached
- **Flag if:** The comment would read exactly the same if posted on a different thread about a similar topic

### 6. Account Risk Check
**Question:** Given the account's karma, age, and posting history — is this comment likely to be filtered, flagged, or shadowbanned?

Risk factors (each increases risk):
- Karma < 50 on a post with > 100 score → moderate risk
- First comment in a subreddit → moderate risk
- Long, polished, multi-paragraph comment from a new account → high risk
- Multiple comments in the same subreddit same day → high risk
- Any external link in first 30 days → block

**Mitigation:** Shorter comment, rougher edges, ask a question instead of making statements, post in a lower-traffic thread first.

### 7. Strategic Fit Check
**Question:** Does this comment serve the warming goal — building genuine community reputation — or does it serve a different goal (brand awareness, product placement, backlinks)?

- Phase 1 goal: Be seen as a knowledgeable, helpful developer. No product mentions.
- **Flag if:** The comment leans toward demonstrating expertise in a specific product area that will later be revealed as "our product"

---

## Verdict Format

```
AUTHENTICITY REVIEW
-------------------
Draft: [2-3 word description]
Target: r/<subreddit> | Post score: <N>
Account: u/<your-account> | Karma: <N> | Day <N> of warming

CHECKS:
1. Value-add:      [PASS / FLAG: reason]
2. AI tells:       [PASS / FLAG: specific phrase or pattern]
3. Specificity:    [PASS / FLAG: reason]
4. Tone match:     [PASS / FLAG: reason]
5. Connection:     [PASS / FLAG: reason]
6. Account risk:   [LOW / MEDIUM / HIGH: reason]
7. Strategic fit:  [PASS / FLAG: reason]

VERDICT: [POST / REVISE / CUT]

POST   — All checks pass. No flags. Account risk LOW or MEDIUM with mitigation.
REVISE — 1-2 flags with clear fixes. Revised draft provided.
CUT    — Multiple flags, or a single HIGH-severity flag. Don't post this comment; find a different thread.

ISSUES:
[List each flagged item with specific fix or cut recommendation]

REVISED DRAFT (if REVISE verdict):
[Cleaned version]
```

---

## Common Fixes

| Problem | Fix |
|---------|-----|
| AI tell opener | Cut opener entirely. Lead with the substance. |
| Three-paragraph essay | Cut to 1-2 paragraphs. Pick the strongest one. |
| Planted specificity | Cut the specific detail, or find a thread where it naturally belongs |
| Too polished | Break a sentence. Use a contraction. Remove one transition. |
| Generic — could be anywhere | Add one direct reference to something specific in the post |
| Too long for account risk | Cut to 100-150 words for new accounts on high-score posts |
| Balanced when dev would have opinion | Pick a side. Remove the hedge. |

---

## Relationship to Other Skills

| Skill | When to use |
|-------|------------|
| **authenticity-reviewer** (this) | Before posting any community comment or short post |
| **text-review-loop** | Before publishing planned brand content (blog, <content-id>, essays) |
| **content-angle** | Before writing — to pick the right angle |
| **adversarial-reviewer** | Before any action with significant blast radius (deploy, external comms, auth changes) |

---

## Example: What We Caught (Reference Run)

**Post:** r/ClaudeCode "Self-improvement Loop" (score 269)
**Draft:** 3-paragraph comment on memory hierarchy half-lives + new Date(undefined) gotcha

**Flags caught:**
- AI tell opener: "The wrap-up pattern is solid" → validation-before-pivot pattern
- Planted specificity: `new Date(undefined)` paragraph — technically real, but disconnected from this thread
- Essay structure: 3 clean paragraphs with smooth transitions — too composed

**Verdict:** REVISE
**Fix:** Cut opener, cut third paragraph, tighten to 2 paragraphs
**Result:** Shorter, rougher, more credible — the half-lives framing stood on its own

---

*Refactored from: adversarial-reviewer (robustness) → content authenticity context*
*Paired with: text-review-loop (heavyweight), content-angle (pre-writing)*
