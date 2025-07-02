# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a GitHub Actions sandbox repository (gaws) used for testing GitHub Actions workflows and configurations. The repository is currently minimal with only basic documentation.

## Repository Structure

- `README.md` - Basic project description in Japanese
- Currently no workflow files or GitHub Actions configurations present

## Workflows

### Deploy Workflow (`.github/workflows/deploy.yml`)
Manual deployment workflow with environment and branch restrictions:
- **Environments**: dev, stg, prd
- **Branch restrictions**:
  - `prd`: main branch only
  - `stg`: main or stg branches only  
  - `dev`: any branch allowed
- **Usage**: Run via GitHub Actions UI with environment and branch inputs

### Four Keys Metrics Workflow (`.github/workflows/four-keys-metrics.yml`)
Automated metrics collection for software delivery performance:
- **Schedule**: Every Saturday at 9:00 AM JST (analyzes previous Monday-Friday)
- **Manual trigger**: Available with week offset parameter
- **Metrics calculated**:
  - Deployment Frequency (main branch merges)
  - Lead Time for Changes (PR creation to merge time)
  - Mean Time to Recovery (hotfix response time)
  - Change Failure Rate (failed deployments percentage)
- **Output**: Creates GitHub issue with weekly metrics report
- **Requirements**: Uses branch strategy defined in README.md

## Development Notes

This appears to be a testing/experimental repository for GitHub Actions. When working with this codebase:

- GitHub Actions workflows are placed in `.github/workflows/` directory
- Test configurations and example workflows may be added as the repository develops
- The project description indicates this is for GitHub Actions testing ("Github Actionsのテスト用ディレクトリ")