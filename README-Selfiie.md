# newman-reporter-slackmsg (Selfiie Fork)

This is a fork of the original [newman-reporter-slackmsg](https://github.com/jackcoded/newman-reporter-slackmsg) customized for Selfiie's specific needs.

## Role in Selfiie API Monitoring

This reporter is a crucial component of the `selfiie-api-monitor` project, which provides automated testing for Selfiie's FHIR API endpoints. For full details, see the `selfiie-api-monitor` documentation.

### How it Works
-   The API monitor is deployed as a Docker container.
-   A Dockerized **Newman** instance executes a comprehensive Postman collection that tests critical API functionality, such as patient record processing, file uploads, and JSON schema validation.
-   Tests are scheduled to run automatically every hour, managed by **Ofelia**, a Docker-based cron job scheduler.
-   This **`newman-reporter-slackmsg`** reporter is used to send real-time notifications to Slack upon test completion.
-   The `--reporter-slackmsg-failuresOnly` option is utilized to ensure that the team is only alerted when tests fail, reducing notification fatigue.

## What's Different in This Fork

This fork includes several enhancements and customizations:

### New Features

**failuresOnly Flag**
- Added `--reporter-slackmsg-failuresOnly` option
- When enabled, Slack messages are only sent when there are test failures
- No messages are sent for successful test runs (unless failures are present and a failuresChannel is configured)
- Useful for reducing notification noise and focusing only on issues that need attention

### Message Formatting Changes

**Cleaner Message Format**
- Changed author name from "Newman Tests" to "Automated test result"
- Removed footer information for cleaner appearance
- Simplified message structure by removing unnecessary dividers
- Removed reporting URL section from the default message layout

**Collection Display**
- Modified to show only the environment name instead of both collection and environment
- Provides more focused information in the Slack message

## Installation

```CLI
npm i -g newman-reporter-slackmsg
```

## Usage

### Standard Usage
```CLI
newman run <collectionFile> -e <environmentFile> --suppress-exit-code -r slackmsg --reporter-slackmsg-webhookurl '<webhookurl>'
```

### Usage with Failures Only
```CLI
newman run <collectionFile> -e <environmentFile> --suppress-exit-code -r slackmsg --reporter-slackmsg-webhookurl '<webhookurl>' --reporter-slackmsg-failuresOnly true
```

### Usage with Channel Override Bot
```CLI
newman run <collectionFile> -e <environmentFile> --suppress-exit-code -r slackmsg --reporter-slackmsg-webhookurl '<https://slack.com/api/chat.postMessage>' --reporter-slackmsg-token '<bearer token>' --reporter-slackmsg-channel '<channel or userid>'
```

## Reporter Options

**Standard Options** (inherited from original)
- `--reporter-slackmsg-webhookurl '<webhookurl>'` - Webhook URL to point to the slack api
- `--reporter-slackmsg-messageSize '<messageSize>'` - e.g 150 (default: 100)
- `--reporter-slackmsg-token '<bearer token>'` - e.g xoxb-XXXXXXXXXXXX-TTTTTTTTTTTTTT
- `--reporter-slackmsg-channel '<channel>'` - e.g #general
- `--reporter-slackmsg-failuresChannel '<channel>'` - e.g. #alerts
- `--reporter-slackmsg-collection '<collectionName>'` - e.g test.json
- `--reporter-slackmsg-environment '<environmentName>'` - e.g env.json
- `--reporter-slackmsg-reportingurl '<URL>'` - e.g https://127.0.1/index.html
- `--reporter-slackmsg-limitFailures '<limitFailures>'` - e.g 5

**New Options** (Selfiie-specific)
- `--reporter-slackmsg-failuresOnly '<boolean>'` - Only send messages when failures occur (default: false)

## Use Cases

### Continuous Integration
Perfect for CI/CD pipelines where you want to be notified only when tests fail:
```CLI
newman run tests.json -e production.json --suppress-exit-code -r slackmsg --reporter-slackmsg-webhookurl '<webhook>' --reporter-slackmsg-failuresOnly true --reporter-slackmsg-channel '#alerts'
```

### Development Monitoring
Use separate channels for different types of notifications:
```CLI
newman run tests.json -e staging.json --suppress-exit-code -r slackmsg --reporter-slackmsg-webhookurl '<webhook>' --reporter-slackmsg-channel '#dev-status' --reporter-slackmsg-failuresChannel '#dev-alerts' --reporter-slackmsg-failuresOnly true
```

## Original Repository

This fork is based on [newman-reporter-slackmsg](https://github.com/jackcoded/newman-reporter-slackmsg) by jackcoded.

## Version

Current version: 1.3.3
