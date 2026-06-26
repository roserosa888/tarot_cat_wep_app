# CLAUDE.md

> **Important:** Do NOT install any packages, tools, or dependencies without explicit user permission first. Always ask the user before installing anything.

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview
This repository serves as a configuration and tooling hub for the `specify` / `speckit` framework. It contains templates, scripts, and configurations for managing software development lifecycles (plans, tasks, and specifications).

## Architecture and Structure
The project is organized around the `.specify` directory:
- `.specify/extensions`: Custom extensions for the system (e.g., `agent-context`).
- `.specify/integrations`: Integration manifests for tools like Copilot and Speckit.
- `.specify/memory`: High-level project governance and guidelines (e.g., `constitution.md`).
- `.specify/scripts/powershell`: Operational scripts for project management:
    - `create-new-feature.ps1`: For initiating new feature development.
    - `setup-plan.ps1`: For creating project implementation plans.
    - `setup-tasks.ps1`: For managing the task list.
    - `check-prerequisites.ps1`: Validates environment setup.
- `.specify/templates`: Standardized templates for project artifacts (`plan-template.md`, `spec-template.md`, `tasks-template.md`).
- `.specify/workflows`: Definition of system workflows.

## Common Development Tasks
Since this is a configuration repository, traditional build/test commands may not apply. Project management is handled via PowerShell scripts in `.specify/scripts/powershell/`.

- **Check Prerequisites**: `powershell .specify/scripts/powershell/check-prerequisites.ps1`
- **Setup Project Plan**: `powershell .specify/scripts/powershell/setup-plan.ps1`
- **Setup Tasks**: `powershell .specify/scripts/powershell/setup-tasks.ps1`
- **Create New Feature**: `powershell .specify/scripts/powershell/create-new-feature.ps1`
