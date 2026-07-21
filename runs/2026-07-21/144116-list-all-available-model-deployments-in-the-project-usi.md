# Bug Bash Run — List all available model deployments in the project using the az CLI (not the SDK), printi

| | |
|---|---|
| **Date** | 2026-07-21 14:41:16 UTC |
| **Interface** | CLI |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ✅ Success — no issues found |
| **Tool Calls** | 5 |

**Scenario:**
> List all available model deployments in the project using the az CLI (not the SDK), printing each deployment name and model version

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 2.66s

**Query:** `Azure AI Foundry az CLI list model deployments project deployment name model version latest docs`

**Result:**

Found 6 result(s):
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Azure Developer CLI (AZD) 1.25.3 or later.  ...Initialize a new hosted agent by using the basic Agent Framework sample i
- [Data, privacy, and security for Foundry Models sold by Azure in Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy)  
  Data, privacy, and security for Models sold by Azure in Mi... This article provides details regarding how data provided 
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Quickstart: Set up Microsoft Foundry resources - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/tutorials/quickstart-create-foundry-resources)  
  Quickstart: Set up Microsoft Foundry resources Summarize this article for me In this article In this quickstart, you cre
- [Configure Claude Code for Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/how-to/configure-claude-code)  
  In this article, you learn how to: Install and configure Claude Code CLI for Microsoft Foundry Set up the Claude Code VS

---

### Step 2 · `search_foundry_docs` · 3.48s

**Query:** `site:learn.microsoft.com/en-us/azure/foundry az cli model deployments list project`

**Result:**

Found 6 result(s):
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Quickstart: Set up Microsoft Foundry resources - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/tutorials/quickstart-create-foundry-resources)  
  Quickstart: Set up Microsoft Foundry resources Summarize this article for me In this article In this quickstart, you cre
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Data, privacy, and security for Foundry Models sold by Azure in Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy)  
  This article provides details regarding how data provided by you to Foundry Models sold by Azure in Microsoft Foundry ar
- [Configure Claude Code for Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/how-to/configure-claude-code)  
  Configure Claude Code for Microsoft Foundry Summarize this article for me In this article Anthropic's Claude Code is an 

---

### Step 3 · `execute_cli_command` · 0.06s

**Command:** `az extension list -o table`

**Result:**

❌ Exit code `-1`

---

### Step 4 · `execute_cli_command` · 0.05s

**Command:** `az account show -o json`

**Result:**

❌ Exit code `-1`

---

### Step 5 · `get_sdk_version_info` · 16.04s

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

## Agent Summary

## Bug Bash Report: List model deployments with az CLI

**Interface Tested:** CLI  
**Outcome:** ❌ Bug Found

### What Was Tested
I attempted to use the Azure CLI to enumerate model deployments in the current Foundry project and print each deployment name plus model version.

### Result
The CLI tool invocation failed before any Azure command could run:

- `az extension list -o table` → `unsupported operand type(s) for +: 'float' and 'str'`
- `az account show -o json` → `unsupported operand type(s) for +: 'float' and 'str'`

Because the CLI wrapper itself errored, I couldn’t verify the deployment-listing command path.

I also checked the latest Foundry docs first, but the doc search results did not surface a clear CLI reference page for listing model deployments in a project.

### Issues Filed
None

### Notes / Next Steps
- This appears to be an issue with the CLI execution wrapper/environment rather than the user’s command.
- SDK versions captured for reference:
  - `azure-ai-projects`: 2.3.0
  - `azure-ai-agents`: 1.1.0
  - `azure-identity`: 1.25.3
  - `agent-dev-cli`: 0.0.1b260427
- If you want, I can retry using a specific `az` command once the CLI execution issue is resolved, or switch to the REST API/SDK path to list deployments.

---

## Issues Filed

_None — scenario completed without filing any issues._

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-21 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*