# Bug Bash Run — List all available model deployments in the project using the az CLI (not the SDK), printi

| | |
|---|---|
| **Date** | 2026-07-21 10:10:53 UTC |
| **Interface** | CLI |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ✅ Success — no issues found |
| **Tool Calls** | 8 |

**Scenario:**
> List all available model deployments in the project using the az CLI (not the SDK), printing each deployment name and model version

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 2.15s

**Query:** `Azure AI Foundry CLI list model deployments project az CLI deployment name model version`

**Result:**

Found 6 result(s):
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Azure Developer CLI (AZD) 1.25.3 or later. Azure CLI and Azure Developer CLI (AZD) installed and authenticated: Step 1: 
- [Quickstart: Set up Microsoft Foundry resources - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/tutorials/quickstart-create-foundry-resources)  
  Quickstart: Set up Microsoft Foundry resources Summarize this article for me In this article In this quickstart, you cre
- [Data, privacy, and security for Foundry Models sold by Azure in Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy)  
  This article provides details regarding how data provided by you to Foundry Models sold by Azure in Microsoft Foundry ar
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Configure Claude Code for Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/how-to/configure-claude-code)  
  In this article, you learn how to: Install and configure Claude Code CLI for Microsoft Foundry Set up the Claude Code VS

---

### Step 2 · `get_sdk_version_info` · 6.22s

**Result:**

| Package | Version |
|---------|---------|
| `azure-ai-projects` | 2.3.0 |
| `azure-ai-agents` | 1.1.0 |
| `azure-identity` | 1.25.3 |
| `azure-mgmt-cognitiveservices` | not installed |
| `openai` | 2.46.0 |
| `azure-ai-evaluation` | not installed |
| `agent-dev-cli` | 0.0.1b260427 |

---

### Step 3 · `fetch_doc_page` · 0.66s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/tutorials/quickstart-create-foundry-resources

**Result:**

📄 **Quickstart: Set up Microsoft Foundry resources - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to Plans Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Set up Microsoft …

---

### Step 4 · `fetch_doc_page` · 1.14s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/deploy-hosted-agent

**Result:**

📄 **Deploy a hosted agent - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to Plans Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Deploy a hosted agent Feedbac…

---

### Step 5 · `execute_cli_command` · 0.04s

**Command:** `az cognitiveservices account project deployment list -h`

**Result:**

❌ Exit code `-1`

---

### Step 6 · `search_foundry_docs` · 1.43s

**Query:** `site:learn.microsoft.com/azure/foundry az cognitiveservices account project deployment list model version`

**Result:**

Found 6 result(s):
- [Deploy models using Azure CLI and Bicep - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/how-to/create-model-deployments)  
   az cognitiveservices account list-models You can deploy the same modelInference keys Manage deployments You can see all
- [Quickstart: Set up Microsoft Foundry resources - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/tutorials/quickstart-create-foundry-resources)  
  Quickstart: Set up Microsoft Foundry resources Summarize this article for me In this article In this quickstart, you cre
- [Endpoints for Microsoft Foundry Models - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/endpoints)  
  Endpoints for Microsoft Foundry Models Summarize this article for me In this article Microsoft Foundry Models enables yo
- [Quickstart: Create a provisioned throughput deployment - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/openai/provisioned-quickstart)  
  Quickstart: Create a provisioned throughput deployment Summarize this article for me In this article Currently viewing: 
- [Accounts - List Models - REST API (Azure Microsoft Foundry)](https://learn.microsoft.com/en-us/rest/api/microsoftfoundry/accountmanagement/accounts/list-models)  
  Accounts - List Models Service: Microsoft Foundry API Version: 2025-06-01 List available Models for the requested Cognit

---

### Step 7 · `fetch_doc_page` · 1.13s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/foundry-models/how-to/create-model-deployments

**Result:**

📄 **Deploy models using Azure CLI and Bicep - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to plan Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Deploy models using Azure CLI …

---

### Step 8 · `execute_cli_command` · 0.03s

**Command:** `az extension add -n cognitiveservices && az cognitiveservices account deployment list -h`

**Result:**

❌ Exit code `-1`

---

## Agent Summary

## Bug Bash Report: List model deployments via az CLI

**Interface Tested:** CLI  
**Outcome:** ❌ Bug Found

### What Was Tested
Attempted to list all available model deployments in the Foundry project using the Azure CLI, with a focus on printing each deployment name and model version.

### Result
I consulted the latest Foundry docs first. The docs indicate:
- `az cognitiveservices account deployment list` should list deployments
- `az cognitiveservices account list-models` lists available models with version metadata

However, the CLI command failed before help output or execution:
- `az cognitiveservices account project deployment list -h`
- `az extension add -n cognitiveservices && az cognitiveservices account deployment list -h`

Both returned:
- `unsupported operand type(s) for +: 'float' and 'str'`

This appears to be a CLI/runtime bug, not a user syntax issue, because it failed even when requesting command help.

### Issues Filed
- None

### Notes / Next Steps
The expected workaround from docs is to use:
- `az cognitiveservices account deployment list` for deployments
- `az cognitiveservices account list-models` for model metadata

But due to the CLI error, I could not complete the requested listing. If you want, I can next try the REST API path or a different az command variant to isolate whether this is an extension installation issue or a core CLI bug.

---

## Issues Filed

_None — scenario completed without filing any issues._

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-21 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*