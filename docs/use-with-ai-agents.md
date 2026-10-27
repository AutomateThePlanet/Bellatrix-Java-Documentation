---
layout: default
title:  "Use with AI Agents"
excerpt: "Let an AI coding agent write BELLATRIX Java tests for web, desktop and mobile with the bellatrix CLI and scaffolded skills."
date:   2026-10-27 06:50:17 +0200
permalink: /use-with-ai-agents/
anchors:
  overview: Overview
  install-the-cli: Install the CLI
  scaffold-the-java-skills: Scaffold the Java Skills
  set-up-the-maven-project: Set Up the Maven Project
  learn-more: Learn More
---
Overview
--------
The BELLATRIX AI agent toolkit lets an AI coding agent (Claude Code, GitHub Copilot, Cursor, Devin, Codex CLI, Gemini CLI for enterprise, Antigravity CLI, Kiro CLI or OpenCode) write BELLATRIX tests for you. It has two parts:
- the **`bellatrix` CLI**, which drives a live app one action at a time (`bellatrix desktop|web|mobile <verb>`) and runs no AI model itself;
- **skills and subagents** that `bellatrix init` writes into your repository, in your agent's native layout. They teach the agent the BELLATRIX Java components, page objects and test structure, and how to resolve stable locators.

The Java skills cover **web, desktop and mobile (Android and iOS)**.

Install the CLI
---------------
The CLI is a .NET global tool, so you need the .NET 8 SDK even for Java projects:
```
dotnet tool install -g Bellatrix.Cli
bellatrix --version
bellatrix help
```
The driver is yours to run: WinAppDriver for desktop, Appium plus a device or emulator for mobile. For web, the CLI starts a local browser driver itself.

Scaffold the Java Skills
------------------------
Run this in the root of your test repository:
```
bellatrix init --agent claude --language java --domains desktop,web,mobile --json
```
Replace `claude` with `copilot`, `cursor`, `devin`, `codex`, `gemini`, `antigravity`, `kiro` or `opencode`, or list several (`--agent claude,cursor`). With a `pom.xml` or `build.gradle` in the root folder, `--language java` is detected automatically. Add `--dry-run` to preview the files.

Set Up the Maven Project
------------------------
The BELLATRIX Java artifacts are not published to Maven Central. Install them from the BELLATRIX-Java source once (Maven on JDK 19):
```
mvn -B -q install -DskipTests -pl bellatrix.web,bellatrix.desktop,bellatrix.android,bellatrix.ios -am
```
Then depend on the module for your domain:
```
<dependency>
    <groupId>solutions.bellatrix</groupId>
    <artifactId>bellatrix.web</artifactId> <!-- or bellatrix.desktop / bellatrix.android / bellatrix.ios -->
    <version>1.0</version>
    <scope>test</scope>
</dependency>
```

Learn More
----------
The full guide — workflow, CLI reference, support matrix per AI agent and known limits — is at [bellatrix.solutions/agent](https://bellatrix.solutions/agent/).
The CLI and the skill templates are open source: [BELLATRIX-Agent on GitHub](https://github.com/AutomateThePlanet/BELLATRIX-Agent).
