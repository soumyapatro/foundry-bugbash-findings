# Bug Bash Run — Create a new Azure AI agent using the azure-ai-agents SDK with no tools and verify it is c

| | |
|---|---|
| **Date** | 2026-07-21 10:04:16 UTC |
| **Interface** | SDK |
| **Agent Model** | `gpt-5-6-sol` |
| **Outcome** | ✅ Success — no issues found |
| **Tool Calls** | 7 |

**Scenario:**
> Create a new Azure AI agent using the azure-ai-agents SDK with no tools and verify it is created successfully by printing its ID and name

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 2.4s

**Query:** `site:learn.microsoft.com/en-us/azure/foundry azure-ai-agents Python create agent no tools AgentsClient create_agent model name`

**Result:**

Found 6 result(s):
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Connect to MCP Server Endpoints for agents - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/model-context-protocol)  
  Connect agents to Model Context Protocol servers Summarize this article for me In this article Connect your Foundry agen
- [Training for Microsoft Foundry](https://learn.microsoft.com/en-us/training/azure/ai-foundry)  
  Microsoft Learn for Microsoft Foundry Innovate with Microsoft Foundry Microsoft Foundry brings together models, tools, a
- [Azure developer documentation](https://learn.microsoft.com/en-us/azure/developer/)  
  Azure developer documentation Develop applications on Azure using Python, JavaScript, Java, .NET, Go, C++, and Rust. Lea

---

### Step 2 · `fetch_doc_page` · 1.44s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code

**Result:**

📄 **Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to plan Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Get started with M…

---

### Step 3 · `search_foundry_docs` · 2.43s

**Query:** `site:learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code "project.agents.create" Python agent`

**Result:**

Found 6 result(s):
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Deploy a hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/deploy-hosted-agent)  
  Deploy a hosted agent Summarize this article for me In this article This article shows you how to deploy a containerized
- [Quickstart: Set up Microsoft Foundry resources - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/tutorials/quickstart-create-foundry-resources)  
  Quickstart: Set up Microsoft Foundry resources Summarize this article for me In this article In this quickstart, you cre
- [Azure developer documentation](https://learn.microsoft.com/en-us/azure/developer/)  
  Azure developer documentation Develop applications on Azure using Python, JavaScript, Java, .NE... Learn through quickst

---

### Step 4 · `search_foundry_docs` · 2.3s

**Query:** `site:learn.microsoft.com/en-us/azure/foundry "azure.ai.agents" "create_agent"`

**Result:**

Found 6 result(s):
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [What is Foundry IQ? - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-foundry-iq)  
  ...is FoundrySummarize this article for me In this article Note Some Foundry IQ features are now generally available, wh
- [Data, privacy, and security for Foundry Models sold by Azure in Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy)  
  This article provides details regarding how data provided by you to Foundry Models sold by Azure in Microsoft Foundry ar
- [Azure Architecture Center - Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)  
  Popular articles Microsoft Foundry chat baseline architecture Develop and optimize a RAG implementation AI agent orchest

---

### Step 5 · `get_sdk_version_info` · 14.05s

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

### Step 6 · `execute_sdk_code` · 0.07s

**Code:**
```python
import os, json, traceback, uuid
from azure.identity import DefaultAzureCredential
from azure.ai.agents import AgentsClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
subscription_id = os.environ.get("AZURE_SUBSCRIPTION_ID", "")
credential = DefaultAzureCredential()

try:
    model = (
        os.environ.get("AZURE_AI_MODEL_DEPLOYMENT_NAME")
        or os.environ.get("AZURE_AI_MODEL_DEPLOYMENT")
        or os.environ.get("AZURE_OPENAI_DEPLOYMENT_NAME")
        or "gpt-4o-mini"
    )
    requested_name = f"bug-bash-no-tools-{uuid.uuid4().hex[:8]}"
    client = AgentsClient(endpoint=endpoint, credential=credential)
    agent = client.create_agent(
        model=model,
        name=requested_name,
        instructions="You are a helpful assistant.",
        tools=[],
    )

    # Verify the service returned a persistent agent with the requested name and no tools.
    fetched = client.get_agent(agent.id)
    tools = getattr(fetched, "tools", None)
    assert fetched.id, "Created agent has no ID"
    assert fetched.name == requested_name, f"Name mismatch: {fetched.name!r} != {requested_name!r}"
    assert tools in (None, []), f"Expected no tools, got: {tools!r}"

    print("SUCCESS: Agent created and fetched successfully")
    print("Agent ID:", fetched.id)
    print("Agent name:", fetched.name)
    print("Tools:", tools)
except Exception:
    print("FAILURE:")
    traceback.print_exc()
```

**Result:**

❌ Exit code `-1`

---

### Step 7 · `execute_sdk_code` · 11.17s

**Code:**
```python
import os, json, traceback, uuid
from azure.identity import DefaultAzureCredential
from azure.ai.agents import AgentsClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
subscription_id = os.environ.get("AZURE_SUBSCRIPTION_ID", "")
credential = DefaultAzureCredential()

try:
    model = (
        os.environ.get("AZURE_AI_MODEL_DEPLOYMENT_NAME")
        or os.environ.get("AZURE_AI_MODEL_DEPLOYMENT")
        or os.environ.get("AZURE_OPENAI_DEPLOYMENT_NAME")
        or "gpt-4o-mini"
    )
    requested_name = f"bug-bash-no-tools-{uuid.uuid4().hex[:8]}"
    client = AgentsClient(endpoint=endpoint, credential=credential)
    agent = client.create_agent(
        model=model,
        name=requested_name,
        instructions="You are a helpful assistant.",
        tools=[],
    )

    fetched = client.get_agent(agent.id)
    tools = getattr(fetched, "tools", None)
    assert fetched.id, "Created agent has no ID"
    assert fetched.name == requested_name, f"Name mismatch: {fetched.name!r} != {requested_name!r}"
    assert tools in (None, []), f"Expected no tools, got: {tools!r}"

    print("SUCCESS: Agent created and fetched successfully")
    print("Agent ID:", fetched.id)
    print("Agent name:", fetched.name)
    print("Tools:", tools)
except Exception:
    print("FAILURE:")
    traceback.print_exc()
```

**Result:**

✅ Exit code `0` · 11.17s

```
SUCCESS: Agent created and fetched successfully
Agent ID: asst_pjdOsFJx0g6Y5M8AvY8vHyTp
Agent name: bug-bash-no-tools-017343b6
Tools: []
```

---

## Agent Summary

## Bug Bash Report: Create an Azure AI agent with no tools

**Interface Tested:** SDK  
**Outcome:** ✅ Success

### What Was Tested
Created an agent using `azure-ai-agents` with:
- A unique name
- An empty tool list (`tools=[]`)
- `DefaultAzureCredential`
- The endpoint from `AZURE_AI_PROJECT_ENDPOINT`

The agent was fetched afterward to verify its ID, name, and empty tool list.

### Result
Agent creation and verification succeeded.

```text
Agent ID: asst_pjdOsFJx0g6Y5M8AvY8vHyTp
Agent name: bug-bash-no-tools-017343b6
Tools: []
```

SDK version tested: `azure-ai-agents 1.1.0`

### Issues Filed
None.

### Notes / Next Steps
The agent remains in the Foundry project and can be deleted when no longer needed.

---

## Issues Filed

_None — scenario completed without filing any issues._

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-21 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*