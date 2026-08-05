# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

**meta-power-user-workflow** is a documentation-only repository. It currently consists of a single file, `README.md`, containing curated notes and reference material on "power user" AI/automation workflows: AI-driven browser tab organization, calendar-driven productivity systems, an overview of the `AGENTS.md` convention and MCP (Model Context Protocol) integration, example n8n/GitHub/MCP automation workflows, Comet browser automation tips, and a list of reusable prompts and external links.

There is no source code, no package manifest (`package.json`, `pyproject.toml`, etc.), no build system, no test suite, and no linter configured. The code snippets inside `README.md` (JavaScript pseudocode, a C# MCP client example, an n8n workflow JSON fragment) are illustrative examples embedded in prose, not part of a runnable project.

## Working in this repo

- Treat this as a Markdown knowledge base: changes are edits to `README.md` (or new Markdown files), not application code.
- There is nothing to build, lint, or test — there is no toolchain to invoke.
- When adding content, follow the existing structure: a top-level Table of Contents linking to `##` sections, each section containing prose plus optional fenced code examples.
- Keep the Table of Contents in sync with section headings when adding, renaming, or removing sections.
