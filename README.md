# Security Checks

## Table of Contents

- Overview
- Example Consumer Workflow
- Reusable Workflow Architecture
- Input Parameters
- Supported Security Scanners

## Overview

This repository contains a reusable GitHub Actions workflow that performs
security checks on code in pull requests. The workflow runs security scanners
(Semgrep and TruffleHog) on committed code changes and fails if any security
issues are detected.

## Example Consumer Workflow

To use this reusable workflow in your repository, create a workflow file
(e.g., `.github/workflows/security-checks.yml`) that calls it:

```yaml
name: 'Security Checks'

on: pull_request

jobs:
  security-checks:
    uses: garretpatten/security-checks/.github/workflows/security-checks.yml@master
    with:
      semgrep_run: true
      trufflehog_run: true
    secrets: inherit
```

## Reusable Workflow Architecture

This repository contains a reusable workflow located at
`.github/workflows/security-checks.yml`. The workflow is designed to perform
essential security checks across projects by running security scanners on code
changes in pull requests.

The workflow checks only the files that have been changed in the pull request,
making it efficient and focused on the code being reviewed.

## Input Parameters

| Parameter      | Type    | Required | Default | Description                    |
| -------------- | ------- | -------- | ------- | ------------------------------ |
| semgrep_run    | boolean | No       | true    | Whether to run Semgrep scanner |
| trufflehog_run | boolean | No       | true    | Whether to run TruffleHog      |

## Supported Security Scanners

### Semgrep

Semgrep is a fast, open-source static analysis tool for finding bugs and
enforcing code standards. It checks code for security vulnerabilities, bugs,
and anti-patterns across multiple languages. It checks the following file
types:

- All supported languages (Python, JavaScript, TypeScript, Java, Go, C/C++,
  and more)

Semgrep will fail if it detects any security vulnerabilities or issues in the
code. The workflow uses Semgrep's `--config=auto` flag, which automatically
selects the appropriate rules based on the languages detected in your
repository.

### TruffleHog

TruffleHog is a secrets scanning tool that detects exposed credentials and
secrets in code repositories. It checks the following file types:

- All files in the repository

TruffleHog will fail if it detects any verified secrets or credentials in the
code. The workflow uses the `--only-verified` flag to reduce false positives
by only reporting secrets that have been verified as active. It also respects
`.truffleignore` files to exclude specific paths from scanning.
