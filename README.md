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
