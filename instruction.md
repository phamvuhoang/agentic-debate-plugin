Below is a complete repo you can copy as-is.

## Repo tree

```text
agentic-debate-plugin/
├─ README.md
├─ LICENSE
├─ skills/
│  └─ agentic-debate/
│     └─ SKILL.md
├─ .claude-plugin/
│  ├─ plugin.json
│  └─ marketplace.json
├─ .codex-plugin/
│  └─ plugin.json
└─ .agents/
   └─ plugins/
      └─ marketplace.json
```

---

## `README.md`

````md
# Agentic Debate Plugin

Structured adversarial debate for high-stakes technical and strategic decisions.

This plugin provides a reusable skill called `agentic-debate` that forces multi-perspective challenge rounds, topic arbitration, and synthesis before reaching a recommendation.

## What it does

Use this when simple pros/cons are not enough.

The skill will:
- define the subject
- select conflicting participants
- run challenge rounds
- group arguments by topic
- arbitrate contested topics
- synthesize a recommendation
- surface open questions

## Repository Structure

```text
agentic-debate-plugin/
├─ README.md
├─ LICENSE
├─ skills/
│  └─ agentic-debate/
│     └─ SKILL.md
├─ .claude-plugin/
│  ├─ plugin.json
│  └─ marketplace.json
├─ .codex-plugin/
│  └─ plugin.json
└─ .agents/
   └─ plugins/
      └─ marketplace.json
````

## Install in Claude Code

### Local testing

Run Claude Code with the plugin directory:

```bash
claude --plugin-dir .
```

Then invoke the skill from inside Claude Code.

### Marketplace-style install

1. Push this repo to GitHub.
2. In Claude Code, add the marketplace:

```text
/plugin marketplace add https://github.com/YOUR_GITHUB_USERNAME/agentic-debate-plugin
```

3. Install the plugin:

```text
/plugin install agentic-debate@YOUR_MARKETPLACE_NAME
```

## Install in Codex

This repo includes Codex-compatible plugin packaging.

Today, the simplest setup is to use it via a local or curated marketplace.

### Local repo-based setup

1. Clone this repo somewhere accessible.
2. Point your Codex marketplace entry to this repo path.
3. Open the Plugins UI or CLI plugin management and install `agentic-debate`.

## Usage

Ask for a debate explicitly, or use it when evaluating competing approaches.

Example prompts:

* "Debate monolith vs microservices for our payments system."
* "Stress-test this architecture before recommending it."
* "Challenge this proposal from security, operability, and cost perspectives."

## Skill Name

* Plugin name: `agentic-debate`
* Skill name: `agentic-debate`

## Publish Checklist

* [ ] Replace `YOUR_NAME`
* [ ] Replace `YOUR_GITHUB_USERNAME`
* [ ] Replace `YOUR_MARKETPLACE_NAME`
* [ ] Push to a public GitHub repo
* [ ] Test local install in Claude
* [ ] Test local install in Codex

## License

MIT

````

---

## `LICENSE`

```text
MIT License

Copyright (c) 2026 YOUR_NAME

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
````

---

## `skills/agentic-debate/SKILL.md`

````md
---
name: agentic-debate
description: Use when comparing 2 or more concrete approaches and a rigorous multi-perspective adversarial debate is needed before recommending one. Not for brainstorming, open-ended ideation, or simple pros/cons lists.
---

# Agentic Debate

A structured adversarial reasoning technique for high-stakes decisions. Multiple named perspectives challenge each other across rounds, contested topics are arbitrated, and findings are synthesized into a verdict.

Use this when listing pros/cons is not enough — when the stakes are high enough that a single perspective could miss critical blind spots.

<HARD-GATE>
Do NOT reach a conclusion or recommend an approach until arbitration is complete for all contested topics. Every participant must have had a chance to challenge at least one other participant before arbitration begins.
</HARD-GATE>

## When to Use

- Comparing 2+ competing architectures, designs, or strategies
- Technical decisions with significant long-term consequences
- When the user asks to "debate", "stress-test", or "challenge" an approach
- When a prior recommendation was challenged or felt one-sided

**NOT for:** Open-ended exploration or ideation (use brainstorming instead). Debate assumes there are defined positions that can conflict.

## Checklist

You MUST create a task for each of these items and complete them in order:

1. **Define the subject** — one-sentence statement of what is being decided
2. **Select participants** — 2–5 named perspectives with distinct, conflicting stakes
3. **Run challenge rounds** — each participant challenges others on specific named topics
4. **Group by topic** — cluster challenges; mark contested vs uncontested
5. **Arbitrate each contested topic** — verdict with confidence level and rationale
6. **Synthesize** — recommendation that integrates the strongest points from all sides
7. **Present transcript** — structured output to user with open questions

## Process Flow

```dot
digraph agentic_debate {
    "Define subject" [shape=box];
    "Select participants (2-5)" [shape=box];
    "Round: each challenges others" [shape=box];
    "Rebuttal needed?" [shape=diamond];
    "Group challenges by topic" [shape=box];
    "Arbitrate each contested topic" [shape=box];
    "Open questions change verdicts?" [shape=diamond];
    "Synthesize findings" [shape=box];
    "Present transcript" [shape=doublecircle];

    "Define subject" -> "Select participants (2-5)";
    "Select participants (2-5)" -> "Round: each challenges others";
    "Round: each challenges others" -> "Rebuttal needed?";
    "Rebuttal needed?" -> "Round: each challenges others" [label="yes"];
    "Rebuttal needed?" -> "Group challenges by topic" [label="no"];
    "Group challenges by topic" -> "Arbitrate each contested topic";
    "Arbitrate each contested topic" -> "Open questions change verdicts?";
    "Open questions change verdicts?" -> "Arbitrate each contested topic" [label="dig deeper"];
    "Open questions change verdicts?" -> "Synthesize findings" [label="no"];
    "Synthesize findings" -> "Present transcript";
}
````

## The Process

### 1. Define the Subject

One sentence. The specific decision, question, or proposal under examination.

> "Subject: Whether to adopt a microservices architecture for the payments system."

### 2. Select Participants

2–5 named perspectives with genuinely conflicting stakes. These are roles or lenses, not real people.

**Good participants:** `performance` · `maintainability` · `security` · `cost` · `operability` · `developer-experience` · `short-term` · `long-term` · `user-needs` · `business-risk`

**Rule:** If two participants would always agree, merge them. Every participant must have a stake that could conflict with at least one other.

### 3. Run Challenge Rounds

Each participant challenges at least one other on a specific, named topic. A valid challenge must:

* Name the topic explicitly (`topic: "data_consistency"`)
* State what position it contests
* Give a concrete argument
* Declare confidence: **high** / **medium** / **low**

```text
[Round 1]
performance → maintainability  |  topic: deployment_complexity
  "Microservices introduce distributed tracing overhead that monoliths avoid.
   Teams spend substantial debug time on network issues that don't exist locally."
  confidence: high

maintainability → performance  |  topic: deployment_complexity
  "Monolith coupling forces coordinated deploys that block independent team velocity.
   At scale, this becomes the bottleneck, not network overhead."
  confidence: medium
```

Run Round 2 if any challenge was met with a credible counter-argument that materially changes the picture.

### 4. Group by Topic

Cluster all challenges under shared topic labels. Topics with challenges from multiple participants are **contested**; single-challenger topics are **uncontested**.

### 5. Arbitrate Each Contested Topic

For each contested topic:

```text
topic: deployment_complexity
  winner: performance  |  confidence: moderate (65%)
  rationale: Distributed tracing overhead is real; tooling has improved but the
             operational burden is non-trivial for smaller teams.
  open_question: What is the current team size and target deployment frequency?
```

Confidence levels:

* **Strong** (>75%): one side's argument is clearly better-evidenced
* **Moderate** (60–75%): one side leads but the other raises valid conditions
* **Contested** (<60%): genuinely depends on unknowns — surface the open questions

### 6. Synthesize

2–4 sentences that:

1. State the overall recommendation, or explain why no clear winner exists
2. Acknowledge the strongest dissenting point
3. List conditions under which the verdict reverses
4. Call out open questions that must be resolved before committing

### 7. Present Transcript

Use this structure:

```text
## Debate: <subject>

### Participants
- <id>: <one-line stance>

### Challenge Rounds
[formatted rounds]

### Verdicts by Topic
[arbitration for each contested topic]

### Synthesis
[recommendation + strongest dissent + reversal conditions]

### Open Questions
[what would change these verdicts]
```

## Key Principles

* **Adversarial by design** — participants must challenge, not just list alternatives from their corner
* **Named topics are required** — vague challenges cannot be arbitrated; name the specific tension
* **Confidence is explicit** — never leave it implicit; it drives arbitration weight
* **Synthesis transcends sides** — the conclusion integrates the strongest points from all participants
* **Open questions belong in the output** — unresolved questions are findings, not failures

````

---

## `.claude-plugin/plugin.json`

```json
{
  "name": "agentic-debate",
  "description": "Structured adversarial debate skill for high-stakes technical and strategic decisions.",
  "version": "1.0.0",
  "author": {
    "name": "YOUR_NAME"
  },
  "homepage": "https://github.com/YOUR_GITHUB_USERNAME/agentic-debate-plugin",
  "repository": "https://github.com/YOUR_GITHUB_USERNAME/agentic-debate-plugin",
  "license": "MIT"
}
````

---

## `.claude-plugin/marketplace.json`

```json
{
  "name": "YOUR_MARKETPLACE_NAME",
  "owner": {
    "name": "YOUR_NAME"
  },
  "plugins": [
    {
      "name": "agentic-debate",
      "source": "./",
      "description": "Structured adversarial debate skill for high-stakes technical and strategic decisions."
    }
  ]
}
```

---

## `.codex-plugin/plugin.json`

```json
{
  "name": "agentic-debate",
  "version": "1.0.0",
  "description": "Structured adversarial debate skill for high-stakes technical and strategic decisions.",
  "author": "YOUR_NAME",
  "homepage": "https://github.com/YOUR_GITHUB_USERNAME/agentic-debate-plugin",
  "repository": "https://github.com/YOUR_GITHUB_USERNAME/agentic-debate-plugin",
  "license": "MIT",
  "keywords": [
    "debate",
    "architecture",
    "decision-making",
    "reasoning"
  ],
  "skills": "./skills/",
  "interface": {
    "displayName": "Agentic Debate",
    "shortDescription": "Stress-test decisions with structured multi-perspective debate.",
    "longDescription": "Runs a structured adversarial debate with named participants, challenge rounds, topic arbitration, and synthesis before reaching a recommendation.",
    "developerName": "YOUR_NAME",
    "category": "Productivity",
    "capabilities": [
      "skills"
    ],
    "websiteURL": "https://github.com/YOUR_GITHUB_USERNAME/agentic-debate-plugin"
  }
}
```

---

## `.agents/plugins/marketplace.json`

```json
{
  "name": "YOUR_MARKETPLACE_NAME",
  "interface": {
    "displayName": "YOUR_MARKETPLACE_NAME"
  },
  "plugins": [
    {
      "name": "agentic-debate",
      "source": {
        "source": "local",
        "path": "./plugins/agentic-debate-plugin"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Productivity"
    }
  ]
}
```

---

## Quick replace checklist

Replace these strings everywhere:

```text
YOUR_NAME
YOUR_GITHUB_USERNAME
YOUR_MARKETPLACE_NAME
```

Example:

```text
YOUR_NAME = Henry Hoang Pham
YOUR_GITHUB_USERNAME = phamvuhoang
YOUR_MARKETPLACE_NAME = agentic-debate-plugins
```

Then your Claude install flow becomes:

```text
/plugin marketplace add https://github.com/phamvuhoang/agentic-debate-plugin
/plugin install agentic-debate@agentic-debate-plugins
```

