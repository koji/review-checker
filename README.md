# Review Checker

A GitHub Actions workflow that automatically finds pull requests where you're requested as a reviewer and sends notifications to Slack.

## Overview

This workflow searches for open PRs in a specified repository that are awaiting your review and sends a summary to a Slack channel via webhook.

## Features

- 🔍 Searches for PRs with pending review requests
- 📬 Sends formatted notifications to Slack
- ⚙️ Configurable target repository and username
- 🕐 Can run on-demand or on a schedule

## Setup

### 1. Required Secrets

Configure the following secrets in your repository settings (`Settings` → `Secrets and variables` → `Actions`):

- **`GH_TOKEN`**: GitHub Personal Access Token with `repo` and `read:org` permissions
- **`SLACK_WEBHOOK_URL`**: Slack incoming webhook URL for notifications

### 2. Configuration

Edit the workflow file `.github/workflows/find-reviews.yml` to customize:

```yaml
env:
  TARGET_REPO: 'Opentrons/opentrons'  # Repository to search
  MY_USERNAME: 'koji'                  # Your GitHub username
```

### 3. Schedule (Optional)

To enable automatic daily checks, uncomment the schedule section in the workflow:

```yaml
schedule:
  - cron: '0 1 * * *'  # Runs daily at 1:00 AM UTC
```

## Usage

### Manual Trigger

1. Go to the **Actions** tab in your repository
2. Select **Find My Review Requests** workflow
3. Click **Run workflow**

### Automatic Trigger

If the schedule is enabled, the workflow runs automatically at the configured time.

## How It Works

1. Uses GitHub CLI (`gh`) to search for open PRs with pending review requests
2. Formats the results as a list with PR numbers, titles, and URLs
3. Builds a JSON payload for Slack
4. Sends the notification using the Slack GitHub Action

## Notification Format

When PRs are found:
```
🔔 **Review Requests for @koji in Opentrons/opentrons**

- #123: Fix bug in API handler
- #456: Add new feature
```

When no PRs are found:
```
✅ No review requests found for @koji in Opentrons/opentrons.
```

## Permissions

The workflow requires the following permissions:
- `pull-requests: read` - To search and read PR information
- `contents: read` - To access repository content

## Requirements

- GitHub CLI (`gh`) - Pre-installed on GitHub-hosted runners
- Slack incoming webhook configured in your workspace
