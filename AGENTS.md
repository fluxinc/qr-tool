# Repository Guidelines

## Project Structure & Module Organization
The entry point `qr-tool.ps1` drives a three-stage pipeline implemented in `lib/`, where `stage-1.ps1`, `stage-2.ps1`, and `stage-3.ps1` handle ingestion, study discovery, and move requests. Runtime scratch space lives in `cache/`; keep its subdirectories writable but out of version control. Configuration is managed through `config.ps1` (copy from `config.example.ps1` when onboarding) and the fo-dicom cmdlets are compiled from `FoDicomCmdlets/`, producing binaries under `FoDicomCmdlets/bin/Release`.

## Build, Test, and Development Commands
Run `msbuild FoDicomCmdlets/FoDicomCmdlets.sln /p:Configuration=Release` to refresh the custom cmdlets before exercising the tool. Execute `pwsh -ExecutionPolicy Bypass -File qr-tool.ps1` for a single pass through the pipeline, or add `-StartWorklistQuery` to poll the modality worklist continuously. Use `pwsh -NoExit -File qr-tool.ps1` during debugging so you can inspect global state after the run.

## Coding Style & Naming Conventions
PowerShell modules use four-space indentation, `PascalCase` function names (e.g., `Do-Stage1`), and descriptive verbs aligned with approved PowerShell terminology. Prefer explicit parameter names, avoid aliases in scripts meant for automation, and keep logging routed through the helpers in `logging.ps1`. C# code should follow standard .NET conventions with `PascalCase` types and methods, `camelCase` locals, and nullable reference annotations where applicable.

## Testing Guidelines
Automated coverage is still being established; new contributions should add Pester tests (place them under `tests/` and mirror module names, e.g., `stage-1.Tests.ps1`) and, for cmdlets, xUnit tests under `FoDicomCmdlets.Tests/`. At minimum, perform a smoke test by dropping a sample `.dcm` into `cache/incoming-stored-items/` and running the main script while watching `cache/logs/`. Document any manual test steps in your PR description.

## Commit & Pull Request Guidelines
Commit history favors concise, sentence-case subjects such as `Update configuration files and enhance README documentation`; keep summaries under 72 characters and expand on details in the body if needed. Reference relevant issues and note whether configuration changes are required. Pull requests should describe the workflow touched, list test evidence, and include screenshots or log excerpts when altering runtime behavior.

## Agent Workflow Notes (Optional)
This repository is wired for Task Master AI. Use `task-master next` to surface the next queued task, `task-master show <id>` for requirements, and log progress with `task-master update-subtask`. After finishing a task, run `task-master set-status --id=<id> --status=done` so downstream work queues stay accurate.
