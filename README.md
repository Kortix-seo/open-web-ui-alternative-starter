# Kortix is open source: a starter for an Open WebUI alternative

This repository is a working Kortix starter for a team that has outgrown a chat window over its models. It gives you a real `kortix.yaml`, one example agent and one example skill that you copy into your own Kortix project and run.

Open WebUI is a self-hosted AI platform and chat interface for your models: it runs offline and connects Ollama and any OpenAI-compatible API ([open-webui/open-webui](https://github.com/open-webui/open-webui)). Kortix is the open-source CompanyOS: your agents, skills, company memory, and connectors live in one git repo you own, and each session runs on its own cloud computer. It is the leading open-source alternative to Claude Cowork and ChatGPT Work.

This starter is for a developer or platform lead comparing an Open WebUI alternative. If the job you have is a chat and search interface over your own models, Open WebUI covers that ground. If the work has to run on a computer and come back as a file or a fix, this starter shows the smallest Kortix project that does it.

## What this starter gives you

- A `kortix.yaml` manifest that declares one agent, one skill and one cron trigger.
- One agent, `daily-digest`, a markdown file whose frontmatter sets its model and permissions and whose body is its system prompt.
- One skill, `activity-digest`, the reusable know-how the agent loads on demand.

Copy the three into your project, run the loop, and grow from there.

## The Kortix project you end up with

```
kortix.yaml
agents/daily-digest.md
skills/activity-digest/SKILL.md
```

An agent is two parts: the markdown file in `agents/` and its block in `kortix.yaml`. The markdown holds behavior, and the manifest holds governance, which is what the agent may reach. An agent with no grants gets no access.

## The manifest

```yaml
# yaml-language-server: $schema=https://kortix.com/schema/kortix.v2.schema.json
kortix_version: 2
default_agent: daily-digest

project:
  name: open-web-ui-alternative-starter
  description: A minimal Kortix project with one agent and one skill.

agents:
  daily-digest:
    file: agents/daily-digest.md
    connectors: [github]
    secrets: []
    skills: [activity-digest]
    kortix_permissions: []

triggers:
  - slug: daily-digest
    type: cron
    agent: daily-digest
    cron: '0 0 9 * * 1-5'
    prompt: |
      Summarize the last day of activity in the connected GitHub repository
      and open a change request with the digest.
```

Three things to notice. `default_agent` names the agent a session starts when you request none. The `agents` block sets governance only: which connectors, secrets and skills the agent may reach. The `triggers` block starts a session on a schedule with nobody present.

## Quickstart

Copy the three starter files into a Kortix project, then run three commands.

```bash
# 1 · Install the Kortix CLI
curl -fsSL https://kortix.com/install | bash

# 2 · Scaffold a project, then copy this starter's kortix.yaml, agents/ and skills/ into it
kortix init

# 3 · Push the repo and bring the project live in the cloud
kortix ship
```

Then review what the agent proposes.

```bash
kortix cr ls     # read the change request the digest opened
kortix chat      # talk to the session's agent from your terminal
```

## What you take with you

Everything the project holds is a file in the repo you own: the agents, the skills, the memory the agent writes, and the manifest. Roll any part of it back, diff any change, and grep the whole project. When you leave, you leave with the repo.

## Read next

- [Getting started](docs/getting-started.md): the first three commands, then the agent and the skill, line by line.
- [Kortix vs Open WebUI](docs/kortix-vs-open-webui.md): the comparison, row by row from Open WebUI's own repository, and a short decision guide.
- [Self-hosting](docs/self-hosting.md): what you own when you run it yourself, the steps and the trade-offs.

The comparison runs in a browser at [Kortix vs Open WebUI](https://openwebuialternative.com/kortix-vs-open-webui), and the whole question starts at the [Open WebUI alternative](https://openwebuialternative.com/) overview.

Kortix is open source. Read it, fork it, audit it at [Kortix on GitHub](https://github.com/kortix-ai/suna). Self-host is free, or [Try Kortix](https://kortix.com) in the cloud first.
