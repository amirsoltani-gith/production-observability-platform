# Contributing Guide

Thank you for your interest in contributing to the Production Observability Platform.

This repository is maintained as a production-inspired engineering project. Every contribution should prioritize readability, maintainability, and long-term quality over short-term speed.

## Development Principles

Before opening a Pull Request, make sure your contribution:

* Solves a real problem.
* Keeps the project simple.
* Follows existing project conventions.
* Includes documentation updates when necessary.
* Does not introduce unnecessary complexity.

## Branch Strategy

Use Git Flow with short-lived feature branches created from the develop branch.
Do not commit directly to the main branch for feature development.

Examples:

* feature/prometheus
* feature/loki
* feature/grafana
* docs/update-readme

Do not commit directly to the main branch for feature development.

## Commit Messages

Follow the Conventional Commits specification for all commits.

Examples:

* feat: add Prometheus deployment
* fix: correct Grafana datasource
* docs: update architecture documentation
* refactor: simplify Docker Compose configuration

## Documentation

Documentation is considered part of the project.

Whenever architecture, infrastructure, or deployment changes, update the related documentation in the docs/ directory.
Documentation updates should be included in the same Pull Request whenever possible.

## Code Quality

Before submitting changes:

* Review your own code.
* Remove unused files.
* Avoid committing secrets.
* Keep commits focused and atomic.

## Engineering Philosophy

The objective of this repository is not only to deploy software, but also to demonstrate sound engineering practices, operational excellence, and production-inspired design.