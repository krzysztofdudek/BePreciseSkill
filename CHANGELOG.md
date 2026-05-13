# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.0] - 2026-05-13

### Added
- Initial release of the `be-precise` skill (`skills/be-precise/SKILL.md`). Pushes the agent toward asking rather than guessing whenever the spec is silent on a hit case, contradicts itself, or tempts a workaround. Five core rules, an Ask/Proceed table, and an explicit list of red-flag thoughts.
- Claude Code plugin scaffolding: `.claude-plugin/plugin.json` (manifest) and `.claude-plugin/marketplace.json` (single-plugin marketplace listing). Installable via `/plugin marketplace add krzysztofdudek/BePreciseSkill` then `/plugin install be-precise@be-precise-marketplace`. Single-file drop-in works for any agent that reads markdown skills.
- MIT license, README, CLAUDE.md with versioning workflow.

[Unreleased]: https://github.com/krzysztofdudek/BePreciseSkill/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/krzysztofdudek/BePreciseSkill/releases/tag/v0.1.0
