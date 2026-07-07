---
name: azurerm-acctest
description: Run terraform-provider-azurerm acceptance tests locally with mitmproxy traffic capture
license: Complete terms in LICENSE.txt
---

# AzureRM Acceptance Test

Use this skill when running [terraform-provider-azurerm](https://github.com/hashicorp/terraform-provider-azurerm) acceptance tests locally.

## Quick Start

### 1. Export the Required Test Environment

Export the variables required by the acceptance test harness before running tests. Replace the placeholder values with the values from your local Azure configuration:

```bash
export ARM_TENANT_ID="..."
export ARM_SUBSCRIPTION_ID="..."
export ARM_CLIENT_ID="..."
export ARM_CLIENT_SECRET="..."
export ARM_ENVIRONMENT=public
export ARM_TEST_LOCATION=westus3
export ARM_TEST_LOCATION_ALT=uksouth
export ARM_TEST_LOCATION_ALT2=eastasia
export ARM_RESOURCE_PROVIDER_REGISTRATIONS=none
export TF_ACC=true
export TF_LOG=INFO
export HTTP_PROXY=127.0.0.1:7070
export HTTPS_PROXY=127.0.0.1:7070
```

`ARM_CLIENT_ID` and `ARM_CLIENT_SECRET` are the service principal credentials used to authenticate with Azure. You can create a service principal using the Azure CLI:

```bash
az ad sp create-for-rbac --name "<myname>-azurerm-acctest" --role="Contributor" --scopes="/subscriptions/<subscription_id>"
```

The resulting json have `appId` and `password` values that correspond to `ARM_CLIENT_ID` and `ARM_CLIENT_SECRET`, respectively.

The `ARM_TEST_LOCATION`, `ARM_TEST_LOCATION_ALT`, and `ARM_TEST_LOCATION_ALT2` values might have to be adjusted depending on availability of resources being tested.

Store these values in `~/.config/azurerm-acctest/.env` for future reuse.

### 2. Start mitmproxy

Check if mitmproxy is already running on port 7070:

```bash
lsof -nP -iTCP:7070 -sTCP:LISTEN
```

If nothing is listening, start mitmproxy:

```bash
mitmproxy -p 7070 --set console_mouse=false
```

Create a temporary directory for traffic captures and logs:

```bash
mkdir -p /tmp/azurerm-acctest
```

### 3. Run a Targeted Acceptance Test

Use `make acctests` with the `SERVICE` value and a regex-based `TESTARGS` filter:

```bash
make acctests SERVICE=monitor TESTARGS='-run="^TestAccMonitorDataCollectionRule_kindDirectToStore$$"'
```

This runs a single test from:

```text
internal/services/monitor/monitor_data_collection_rule_resource_test.go
```

The `SERVICE` value is derived from the third path segment of the test file path.

### 4. Preserve Test Output

Keep the test output for future investigation:

```bash
make acctests SERVICE=monitor TESTARGS='-run="^TestAccMonitorDataCollectionRule_kindDirectToStore$$"' 2>&1 | tee /tmp/azurerm-acctest/acctest.log
```
