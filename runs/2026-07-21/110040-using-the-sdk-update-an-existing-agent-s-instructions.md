# Bug Bash Run — Using the SDK, update an existing agent's instructions to a new value and then retrieve th

| | |
|---|---|
| **Date** | 2026-07-21 11:00:40 UTC |
| **Interface** | SDK |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ✅ Success — no issues found |
| **Tool Calls** | 8 |

**Scenario:**
> Using the SDK, update an existing agent's instructions to a new value and then retrieve the agent to confirm the instructions were updated correctly

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 2.45s

**Query:** `Azure AI Foundry SDK update agent instructions retrieve agent instructions azure-ai-projects agents documentation`

**Result:**

Found 6 result(s):
- [Update the model for a hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/update-hosted-agent-model)  
  Update the model for a hosted agent ... Change the AI model your hosted agent uses after initial deployment. Update the 
- [Quickstart: Deploy your own code as a hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-deploy-own-code)  
  Quickstart: Deploy your own code as a hosted agent Summarize this article for me In this article In Deploy your first ho
- [How to use an existing AI Search index with the Azure AI Search tool (classic) - Microsoft Foundry (classic) portal](https://learn.microsoft.com/en-us/azure/foundry-classic/agents/how-to/tools-classic/azure-ai-search)  
  Azure AI Search tool (classic) Summarize this article for me In this article Note This document refers to the Microsoft 
- [Connect an Azure AI Search index to Foundry agents - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/ai-search)  
  Connect an Azure AI Search index to Foundry agents Summarize this article for me In this article Tip For a managed knowl
- [What's new in Foundry Agent Service? (classic) - Microsoft Foundry (classic) portal](https://learn.microsoft.com/en-us/azure/foundry-classic/agents/whats-new)  
  What's new in Foundry Agent Service (classic) Summarize this article for me In this article Note This document refers to

---

### Step 2 · `search_foundry_docs` · 2.77s

**Query:** `azure-ai-projects agent update instructions get agent update_agent instructions SDK`

**Result:**

Found 6 result(s):
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [What is Foundry IQ? - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-foundry-iq)  
  What is Foundry IQ. Summarize this article for me In this article Note Some Foundry IQ features are now generally availa
- [Azure developer documentation](https://learn.microsoft.com/en-us/azure/developer/)  
  Azure developer documentation Develop applications on Azure using Python, JavaScript, Java, .NET, Go, C++, and Rust. Wha
- [Microsoft Agent Framework Agent Types - Microsoft Foundry](https://learn.microsoft.com/en-us/agent-framework/agents/)  
  Microsoft Agent Framework agent types Summarize this article for me In this article The Microsoft Agent Framework provid

---

### Step 3 · `fetch_doc_page` · 0.71s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent

**Result:**

📄 **Quickstart: Deploy your first hosted agent - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to Plans Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Deploy your first…

---

### Step 4 · `execute_sdk_code` · 0.04s

**Code:**
```python
import os, json, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
credential = DefaultAzureCredential()
agent_id = os.environ.get('AZURE_TEST_AGENT_ID', '')
new_instructions = 'You are a concise assistant. Always answer in one sentence.'

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    agents = client.agents
    if not agent_id:
        raise ValueError('AZURE_TEST_AGENT_ID environment variable is required for this test.')

    updated = agents.update_agent(agent_id=agent_id, instructions=new_instructions)
    retrieved = agents.get_agent(agent_id=agent_id)

    result = {
        'updated_agent_id': getattr(updated, 'id', None),
        'updated_instructions': getattr(updated, 'instructions', None),
        'retrieved_agent_id': getattr(retrieved, 'id', None),
        'retrieved_instructions': getattr(retrieved, 'instructions', None),
        'match': getattr(retrieved, 'instructions', None) == new_instructions,
    }
    print('SUCCESS:', json.dumps(result, indent=2, default=str))
except Exception:
    print('FAILURE:')
    traceback.print_exc()
```

**Result:**

❌ Exit code `-1`

---

### Step 5 · `execute_sdk_code` · 0.04s

**Code:**
```python
import os, json, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ['AZURE_AI_PROJECT_ENDPOINT']
credential = DefaultAzureCredential()
agent_id = os.environ.get('AZURE_TEST_AGENT_ID', '')
new_instructions = 'You are a concise assistant. Always answer in one sentence.'

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    agents = client.agents
    print('client_type:', type(client).__name__)
    print('agents_type:', type(agents).__name__)
    print('agent_id_present:', bool(agent_id))
    if not agent_id:
        raise ValueError('AZURE_TEST_AGENT_ID environment variable is required for this test.')
    print('Attempting update...')
    updated = agents.update_agent(agent_id=agent_id, instructions=new_instructions)
    print('Attempting get...')
    retrieved = agents.get_agent(agent_id=agent_id)
    result = {
        'updated_agent_id': getattr(updated, 'id', None),
        'updated_instructions': getattr(updated, 'instructions', None),
        'retrieved_agent_id': getattr(retrieved, 'id', None),
        'retrieved_instructions': getattr(retrieved, 'instructions', None),
        'match': getattr(retrieved, 'instructions', None) == new_instructions,
    }
    print('SUCCESS:', json.dumps(result, indent=2, default=str))
except Exception:
    print('FAILURE:')
    traceback.print_exc()
```

**Result:**

❌ Exit code `-1`

---

### Step 6 · `get_sdk_version_info` · 4.53s

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

### Step 7 · `search_foundry_docs` · 2.52s

**Query:** `unsupported operand type(s) for +: 'float' and 'str' azure-ai-projects 2.3.0 agent update bug`

**Result:**

Found 6 result(s):
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Quickstart: Create a prompt agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/prompt-agent)  
  Quickstart: Create a prompt agent Summarize this article for me In this article In this quickstart, you create a prompt 
- [Training for Microsoft Foundry](https://learn.microsoft.com/en-us/training/azure/ai-foundry)  
  Microsoft Learn for Microsoft Foundry Innovate with Microsoft Foundry Microsoft Foundry brings together models, tools, a
- [Azure Architecture Center - Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)  
  Azure Architecture Center is a catalog of solution ideas, example workloads, reference architectures, technology decisio

---

### Step 8 · `execute_sdk_code` · 0.04s

**Code:**
```python
import traceback
try:
    from azure.ai.projects import AIProjectClient
    import inspect
    print('AIProjectClient init:', inspect.signature(AIProjectClient))
    print('Has agents attr on class:', hasattr(AIProjectClient, 'agents'))
except Exception:
    traceback.print_exc()
```

**Result:**

❌ Exit code `-1`

---

## Agent Summary

## Bug Bash Report: Update existing agent instructions and retrieve agent

**Interface Tested:** SDK  
**Outcome:** ❌ Bug Found

### What Was Tested
I attempted to update an existing agent’s `instructions` value using the Azure AI Projects SDK, then retrieve the agent to confirm the change.

### Result
I consulted the latest Foundry docs first, then tried to execute the SDK test using `azure-ai-projects` with `DefaultAzureCredential` and `AZURE_AI_PROJECT_ENDPOINT`.

However, the SDK execution failed immediately with:

`unsupported operand type(s) for +: 'float' and 'str'`

This happened before the test code could complete, including a follow-up attempt with additional debug logging. Because the failure occurred inside the SDK execution environment, I could not validate the agent update/retrieval flow.

### Issues Filed
- None

### Notes / Next Steps
- SDK versions in this environment:
  - `azure-ai-projects`: `2.3.0`
  - `azure-ai-agents`: `1.1.0`
  - `azure-identity`: `1.25.3`
  - Python: `3.13.14`
- This looks like an environment/runtime issue in `execute_sdk_code` rather than a user-code problem.
- If you want, I can retry the scenario through a different interface, such as REST API or CLI, if appropriate.

---

## Issues Filed

_None — scenario completed without filing any issues._

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-21 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*