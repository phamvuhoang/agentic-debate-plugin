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
```

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
/plugin marketplace add https://github.com/phamvuhoang/agentic-debate-plugin
```

3. Install the plugin:

```text
/plugin install agentic-debate@agentic-debate-plugins
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

* [x] Replace `YOUR_NAME`
* [x] Replace `YOUR_GITHUB_USERNAME`
* [x] Replace `YOUR_MARKETPLACE_NAME`
* [ ] Push to a public GitHub repo
* [ ] Test local install in Claude
* [ ] Test local install in Codex

## License

MIT
