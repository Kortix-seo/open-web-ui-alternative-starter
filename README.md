# Kortix is open source: a starter for an Open WebUI alternative

This repository is a working Kortix starter for a team that has outgrown a chat window over its models. It gives you a real `kortix.yaml`, one example agent and one example skill that you copy into your own Kortix workspace and run.

Open WebUI is a self-hosted AI platform and chat interface for your models: it runs offline and connects Ollama and any OpenAI-compatible API ([open-webui/open-webui](https://github.com/open-webui/open-webui)). Kortix is the open-source CompanyOS: your agents, skills, company memory, and connectors live in one git repo you own, and each session runs on its own cloud computer. It is the leading open-source alternative to Claude Cowork and ChatGPT Work.

This starter is for a developer or platform lead comparing an Open WebUI alternative. If the job you have is a chat and search interface over your own models, Open WebUI covers that ground. If the work has to run on a computer and come back as a file or a fix, this starter shows the smallest Kortix workspace that does it.

## What this starter gives you

- A `kortix.yaml` manifest that declares one agent, one skill and one cron trigger.
- One agent, `daily-digest`: a block in `kortix.yaml` that sets its prompt file and its permission, and a markdown file in `agents/` that holds the prompt.
- One skill, `activity-digest`, the reusable know-how the agent loads on demand.

Copy the three into your workspace, run the loop, and grow from there.

## The Kortix workspace you end up with

```
kortix.yaml
agents/daily-digest.md
skills/activity-digest/SKILL.md
```

An agent is two parts: its block in `kortix.yaml` and the markdown file that block points at. The block holds behavior (the prompt file and the permission) and the grants, which is what the agent may reach. This starter grants `all` on tools, names one skill and one connector, and leaves secrets and Kortix permissions empty; change a line to change what the agent can reach.

## The manifest

```yaml
# yaml-language-server: $schema=https://kortix.com/schema/kortix.schema.json
project:
  name: open-web-ui-alternative-starter
  description: A minimal Kortix project with one agent and one skill.

builtin_agents:
  my_kortix: true

default_agent: daily-digest

skills:
  activity-digest: skills/activity-digest

agents:
  daily-digest:
    description: "Daily digest: summarizes the last day of repository activity and opens a change request with the digest."
    mode: primary
    permission: allow
    prompt_file: agents/daily-digest.md
    tools: all
    skills: [activity-digest]
    connectors: [github]
    secrets: []
    kortix_permissions: []
    workspace: { project_repository: true }

triggers:
  - slug: daily-digest
    type: cron
    agent: daily-digest
    cron: '0 0 9 * * 1-5'
    prompt: |
      Summarize the last day of activity in the connected GitHub repository
      and open a change request with the digest.
```

Three things to notice. `default_agent` names the agent a session starts when you request none. Each `agents` block holds behavior and grants: the prompt file, the permission, and which tools, skills, connectors and secrets the agent may reach. The `triggers` block starts a session on a schedule with nobody present.

## Quickstart

Copy the three starter files into a Kortix workspace, then run three commands.

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

Everything the workspace holds is a file in the repo you own: the agents, the skills, the memory the agent writes, and the manifest. Roll any part of it back, diff any change, and grep the whole workspace. When you leave, you leave with the repo.

## Read next

- [Getting started](docs/getting-started.md): the first three commands, then the agent and the skill, line by line.
- [Kortix vs Open WebUI](docs/kortix-vs-open-webui.md): the comparison, row by row from Open WebUI's own repository, and a short decision guide.
- [Self-hosting](docs/self-hosting.md): what you own when you run it yourself, the steps and the trade-offs.

The comparison runs in a browser at [Kortix vs Open WebUI](https://openwebuialternative.com/kortix-vs-open-webui), and the whole question starts at the [Open WebUI alternative](https://openwebuialternative.com/) overview.

Kortix is open source. Read it, fork it, audit it at [Kortix on GitHub](https://github.com/kortix-ai/CompanyOS). Self-host is free, or [Try Kortix](https://kortix.com) in the cloud first.
