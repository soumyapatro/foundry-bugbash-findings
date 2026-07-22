# Bug Bash Run — Create a new Azure AI agent using the SDK with no tools and verify it is created successfu

| | |
|---|---|
| **Run ID** | `run_20260722_152728` |
| **Backend** | hosted |
| **Date** | 2026-07-22 15:29:14 UTC |
| **Interface** | SDK |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ✅ Success — no issues found |
| **Tool Calls** | 5 |

**Scenario:**
> Create a new Azure AI agent using the SDK with no tools and verify it is created successfully by printing its ID and name

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 2.22s

**Query:** `Azure AI Foundry SDK create agent AgentsClient no tools create_agent default model instructions docs`

**Result:**

Found 6 result(s):
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Quickstart: Set up Microsoft Foundry resources - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/tutorials/quickstart-create-foundry-resources)  
  Quickstart: Set up Microsoft Foundry resources Summarize this article for me In this article In this quickstart, you cre
- [Connect to MCP Server Endpoints for agents - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/model-context-protocol)  
  Connect agents to Model Context Protocol servers Summarize this article for me In this article Connect your Foundry agen
- [Azure developer documentation](https://learn.microsoft.com/en-us/azure/developer/)  
  Azure developer documentation Develop applications on Azure using Python, JavaScript, Java, .NET, Go, C++, and Rust. Wha

---

### Step 2 · `get_sdk_version_info` · 28.04s

**Result:**

| Package | Version |
|---------|---------|
| `azure-ai-projects` | error checking |
| `azure-ai-agents` | 1.1.0 |
| `azure-identity` | 1.23.0 |
| `azure-mgmt-cognitiveservices` | not installed |
| `openai` | 2.46.0 |
| `azure-ai-evaluation` | not installed |
| `agent-dev-cli` | 0.0.1b260427 |

---

### Step 3 · `fetch_doc_page` · 1.33s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code

**Result:**

📄 **Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to Plans Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Get started with …

---

### Step 4 · `fetch_doc_page` · 0.13s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent

**Result:**

📄 **Quickstart: Deploy your first hosted agent - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to Plans Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Deploy your first…

---

### Step 5 · `execute_sdk_code` · 9.93s

**Code:**
```python
import os, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.agents import AgentsClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
credential = DefaultAzureCredential()

try:
    with AgentsClient(endpoint=endpoint, credential=credential) as client:
        agent = client.create_agent(model="gpt-4o-mini", name="bugbash-agent-no-tools", instructions="You are helpful.")
        print(f"Agent created: id={agent.id}, name={agent.name}")
        client.delete_agent(agent.id)
        remaining = [a.id for a in client.list_agents()]
        if agent.id not in remaining:
            print(f"Agent deleted: {agent.id} confirmed removed from list")
except Exception:
    print("FAILURE:")
    traceback.print_exc()

```

**Result:**

✅ Exit code `0` · 9.92s

```
Agent created: id=asst_s898pIdSAPzjvKi2TVOC1pqJ, name=bugbash-agent-no-tools
Agent deleted: asst_s898pIdSAPzjvKi2TVOC1pqJ confirmed removed from list
```

---

## Agent Summary

## Bug Bash Report: Create Azure AI agent with no tools

**Interface Tested:** SDK  
**Outcome:** ✅ Success

### What Was Tested
Created a new Azure AI agent using `AgentsClient` in the Azure AI Foundry SDK with no tools configured, then verified creation by printing the agent ID and name.

### Result
The agent was created successfully and returned:
- `id=asst_s898pIdSAPzjvKi2TVOC1pqJ`
- `name=bugbash-agent-no-tools`

Cleanup also succeeded; the agent was deleted and confirmed removed from the list.

### Issues Filed
None

### Notes / Next Steps
No issues were found. SDK version info collected:
- `azure-ai-agents`: `1.1.0`
- `azure-ai-projects`: `error checking`
- `azure-identity`: `1.23.0`
- `openai`: `2.46.0`
- `agent-dev-cli`: `0.0.1b260427`

---

## Issues Filed

_None — scenario completed without filing any issues._

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-22 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*