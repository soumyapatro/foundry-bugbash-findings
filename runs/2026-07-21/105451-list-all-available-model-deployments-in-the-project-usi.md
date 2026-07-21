# Bug Bash Run — List all available model deployments in the project using the az CLI (not the SDK), printi

| | |
|---|---|
| **Date** | 2026-07-21 10:54:51 UTC |
| **Interface** | CLI |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ✅ Success — no issues found |
| **Tool Calls** | 4 |

**Scenario:**
> List all available model deployments in the project using the az CLI (not the SDK), printing each deployment name and model version

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 2.26s

**Query:** `Azure AI Foundry az CLI list model deployments project deployment name model version`

**Result:**

Found 6 result(s):
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Data, privacy, and security for Foundry Models sold by Azure in Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy)  
  This article provides details regarding how data provided by you to Foundry Models sold by Azure in Microsoft Foundry ar
- [Quickstart: Set up Microsoft Foundry resources - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/tutorials/quickstart-create-foundry-resources)  
  Quickstart: Set up Microsoft Foundry resources Summarize this article for me In this article In this quickstart, you cre
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Configure Claude Code for Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/how-to/configure-claude-code)  
  In this article, you learn how to: Install and configure Claude Code CLI for Microsoft Foundry Set up the Claude Code VS

---

### Step 2 · `search_foundry_docs` · 2.71s

**Query:** `site:learn.microsoft.com azure ai foundry cli model deployments list az ai foundry project model deployment`

**Result:**

Found 6 result(s):
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In t... You will: Generate a response f
- [Quickstart: Set up Microsoft Foundry resources - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/tutorials/quickstart-create-foundry-resources)  
  Quickstart: Set up Microsoft Foundry resources Summarize this article for me In this article In this quickstart, you cre
- [Data, privacy, and security for Foundry Models sold by Azure in Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy)  
  This article provides details regarding how data provided by you to Foundry Models sold by Azure in Microsoft Foundry ar
- [Microsoft Foundry Models overview - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/concepts/foundry-models-overview)  
  Microsoft Foundry Models overview Summarize this article for me In this article Microsoft Foundry Models is your one-sto

---

### Step 3 · `execute_cli_command` · 0.07s

**Command:** `az extension list -o table && echo '---' && az ai help`

**Result:**

❌ Exit code `-1`

---

### Step 4 · `execute_cli_command` · 0.04s

**Command:** `az extension list --query "[].{name:name,version:version}" -o json`

**Result:**

❌ Exit code `-1`

---

## Agent Summary

## Bug Bash Report: List model deployments in project via az CLI

**Interface Tested:** CLI  
**Outcome:** ❌ Bug Found

### What Was Tested
I attempted to use the Azure CLI to list available model deployments in the current Microsoft Foundry project and print each deployment name and model version.

### Result
I first consulted the latest Microsoft Foundry docs, but the search results available here did not surface a metadata page with the exact CLI command for listing model deployments. I then tried to inspect installed Azure CLI extensions with:

```bash
az extension list -o table
az extension list --query "[].{name:name,version:version}" -o json
```

Both commands failed immediately in the execution environment with:

```text
unsupported operand type(s) for +: 'float' and 'str'
```

This prevented me from discovering/verifying the correct CLI command and from completing the requested deployment listing.

### Issues Filed
- None

### Notes / Next Steps
- This appears to be an execution-environment CLI issue rather than a Foundry service issue.
- If you want, I can retry once the CLI environment is healthy, or switch to the API path to list deployments if you can provide the project/resource identifiers.

---

## Issues Filed

_None — scenario completed without filing any issues._

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-21 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*