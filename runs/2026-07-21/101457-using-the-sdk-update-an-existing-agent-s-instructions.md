# Bug Bash Run — Using the SDK, update an existing agent's instructions to a new value and then retrieve th

| | |
|---|---|
| **Date** | 2026-07-21 10:14:57 UTC |
| **Interface** | SDK |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ✅ Success — no issues found |
| **Tool Calls** | 9 |

**Scenario:**
> Using the SDK, update an existing agent's instructions to a new value and then retrieve the agent to confirm the instructions were updated correctly

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 2.15s

**Query:** `Azure AI Foundry SDK update agent instructions retrieve agent azure-ai-projects`

**Result:**

Found 6 result(s):
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [What is Foundry IQ? - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-foundry-iq)  
  What is Foundry IQ. Summarize this article for me In this article Note Some Foundry IQ features are now generally availa
- [Azure Architecture Center - Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)  
  Popular articles Microsoft Foundry chat baseline architecture Develop and optimize a RAG implementation AI agent orchest
- [Azure developer documentation](https://learn.microsoft.com/en-us/azure/developer/)  
  Azure developer documentation Develop applications on Azure using Python, JavaScript, Java, .NET, Go, C++, and Rust. Use

---

### Step 2 · `get_sdk_version_info` · 8.01s

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

### Step 3 · `fetch_doc_page` · 0.38s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code

**Result:**

📄 **Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to plan Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Get started with M…

---

### Step 4 · `fetch_doc_page` · 0.38s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent

**Result:**

📄 **Quickstart: Deploy your first hosted agent - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to Plans Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Deploy your first…

---

### Step 5 · `execute_sdk_code` · 0.04s

**Code:**
```python
import os, json, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ['AZURE_AI_PROJECT_ENDPOINT']
credential = DefaultAzureCredential()
new_instructions = 'You are a helpful agent. Always answer in concise bullet points.'

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    # Find an existing agent
    agents = list(client.agents.list_agents())
    print(f'Found {len(agents)} agents')
    if not agents:
        raise RuntimeError('No existing agents found in this project')
    agent = agents[0]
    agent_id = getattr(agent, 'id', None) or getattr(agent, 'agent_id', None)
    print('Using agent:', json.dumps(agent, default=str, indent=2))
    print('Agent ID:', agent_id)

    updated = client.agents.update_agent(agent_id=agent_id, instructions=new_instructions)
    print('UPDATE_RESULT:', json.dumps(updated, default=str, indent=2))

    retrieved = client.agents.get_agent(agent_id=agent_id)
    print('RETRIEVED_AGENT:', json.dumps(retrieved, default=str, indent=2))
    retrieved_instructions = getattr(retrieved, 'instructions', None)
    print('RETRIEVED_INSTRUCTIONS:', retrieved_instructions)
    print('MATCH:', retrieved_instructions == new_instructions)
except Exception:
    print('FAILURE:')
    traceback.print_exc()

```

**Result:**

❌ Exit code `-1`

---

### Step 6 · `search_foundry_docs` · 2.08s

**Query:** `azure-ai-projects list agents get agent update agent instructions SDK update_agent get_agent`

**Result:**

Found 6 result(s):
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [What is Foundry IQ? - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-foundry-iq)  
  What is Foundry IQ. Summarize this article for me In this article Note Some Foundry IQ features are now generally availa
- [Microsoft Agent Framework Agent Types - Microsoft Foundry](https://learn.microsoft.com/en-us/agent-framework/agents/)  
  Microsoft Agent Framework agent types Summarize this article for me In this article The Microsoft Agent Framework provid
- [Work IQ MCP overview (preview)](https://learn.microsoft.com/en-us/microsoft-agent-365/tooling-servers-overview)  
  Work IQ is the intelligence layer that grounds Microsoft 365 Copilot and your agents in real-time, shared context across

---

### Step 7 · `execute_sdk_code` · 0.04s

**Code:**
```python
import os, json, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ['AZURE_AI_PROJECT_ENDPOINT']
credential = DefaultAzureCredential()
new_instructions = 'You are a helpful agent. Always answer in concise bullet points.'

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    agents = list(client.agents.list())
    print(f'Found {len(agents)} agents')
    if not agents:
        raise RuntimeError('No existing agents found in this project')
    agent = agents[0]
    agent_id = getattr(agent, 'id', None) or getattr(agent, 'agent_id', None)
    print('Using agent_id:', agent_id)
    print('BEFORE:', json.dumps(agent, default=str, indent=2))

    updated = client.agents.update(agent_id=agent_id, instructions=new_instructions)
    print('UPDATE_RESULT:', json.dumps(updated, default=str, indent=2))

    retrieved = client.agents.get(agent_id=agent_id)
    print('RETRIEVED_AGENT:', json.dumps(retrieved, default=str, indent=2))
    retrieved_instructions = getattr(retrieved, 'instructions', None)
    print('RETRIEVED_INSTRUCTIONS:', retrieved_instructions)
    print('MATCH:', retrieved_instructions == new_instructions)
except Exception:
    print('FAILURE:')
    traceback.print_exc()

```

**Result:**

❌ Exit code `-1`

---

### Step 8 · `execute_sdk_code` · 0.03s

**Code:**
```python
import os, json, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ['AZURE_AI_PROJECT_ENDPOINT']
credential = DefaultAzureCredential()

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    print('Client created')
    print('Agents attr:', type(client.agents))
    print('Dir contains:', [x for x in dir(client.agents) if 'agent' in x.lower() or x in ['list','get','update']])
except Exception:
    print('FAILURE:')
    traceback.print_exc()

```

**Result:**

❌ Exit code `-1`

---

### Step 9 · `execute_sdk_code` · 0.03s

**Code:**
```python
import traceback
try:
    import azure.ai.projects as p
    print('Imported azure.ai.projects:', p)
except Exception:
    print('FAILURE:')
    traceback.print_exc()

```

**Result:**

❌ Exit code `-1`

---

## Agent Summary

## Bug Bash Report: Update existing agent instructions via SDK

**Interface Tested:** SDK  
**Outcome:** ❌ Bug Found

### What Was Tested
Attempted to use the Azure AI Foundry Python SDK (`azure-ai-projects`) to:
1. Connect to the project using `DefaultAzureCredential`
2. List existing agents
3. Update an existing agent’s `instructions`
4. Retrieve the agent to confirm the update

### Result
The SDK failed immediately with:

```text
unsupported operand type(s) for +: 'float' and 'str'
```

This happened even before agent listing or update logic could execute, including on a minimal import/client-creation test. That suggests a packaging/runtime issue in the SDK environment rather than a mistake in the test code.

### Issues Filed
- None

### Notes / Next Steps
- I consulted the latest Foundry docs first and verified the installed SDK versions.
- The failure appears unrelated to incorrect agent-update parameters; it occurs at SDK initialization/import time.
- Because no workaround was identified from the docs, this should be investigated as an SDK bug.
- If you want, I can next try the same scenario via the REST API or CLI to isolate whether the issue is SDK-specific.

---

## Issues Filed

_None — scenario completed without filing any issues._

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-21 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*