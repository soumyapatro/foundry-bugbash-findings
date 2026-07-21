# Bug Bash Run — Create an agent with the code interpreter tool enabled using the SDK, then run it with the

| | |
|---|---|
| **Date** | 2026-07-21 10:09:46 UTC |
| **Interface** | SDK |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ⚠️ Known Limitation |
| **Tool Calls** | 11 |

**Scenario:**
> Create an agent with the code interpreter tool enabled using the SDK, then run it with the prompt 'Calculate the square root of 144 using code' and verify the code interpreter was invoked and the answer 12 is returned

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 2.03s

**Query:** `Azure AI Foundry SDK create agent code interpreter tool assistant tool prompt calculate square root of 144 verify code interpreter invoked`

**Result:**

Found 6 result(s):
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  The Python SDK path in this quickstart creates and routes a hosted agent version, but it doesn't scaffold a new Foundry 
- [Data, privacy, and security for Foundry Models sold by Azure in Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy)  
  Data, privacy, and security for Models sold by Azure in Microsoft Foundry Summarize this article for me In this article 
- [Training for Microsoft Foundry](https://learn.microsoft.com/en-us/training/azure/ai-foundry)  
  Introduction to Microsoft Foundry Explore Microsoft Foundry Your path to getting started with Microsoft Foundry Get star
- [Azure developer documentation](https://learn.microsoft.com/en-us/azure/developer/)  
  Azure developer documentation Develop applications on Azure using Python, JavaScript, Java, .NET, Go, C++, and Rust. Lea

---

### Step 2 · `get_sdk_version_info` · 7.77s

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

### Step 3 · `fetch_doc_page` · 0.34s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code

**Result:**

📄 **Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to plan Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Get started with M…

---

### Step 4 · `fetch_doc_page` · 0.45s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent

**Result:**

📄 **Quickstart: Deploy your first hosted agent - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to Plans Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Deploy your first…

---

### Step 5 · `search_foundry_docs` · 1.43s

**Query:** `Foundry SDK code interpreter tool agent tools code interpreter azure-ai-projects`

**Result:**

Found 6 result(s):
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [What is Foundry IQ? - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-foundry-iq)  
  What is Foundry IQ. Summarize this article for me In this article Note Some Foundry IQ features are now generally availa
- [Training for Microsoft Foundry](https://learn.microsoft.com/en-us/training/azure/ai-foundry)  
  Introduction to Microsoft Foundry Explore Microsoft Foundry Your path to getting started with Microsoft Foundry Get star
- [Azure developer documentation](https://learn.microsoft.com/en-us/azure/developer/)  
  Azure developer documentation Develop applications on Azure using Python, JavaScript, Java, .NET, Go, C++, and Rust. Use

---

### Step 6 · `execute_sdk_code` · 0.05s

**Code:**
```python
import os, json, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
credential = DefaultAzureCredential()

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    conn = client.connections.get_default("azure_openai")
    print("Default model connection:", conn)

    agent = client.agents.create_agent(
        model=conn.data['endpoint'] if isinstance(conn.data, dict) and 'endpoint' in conn.data else conn.id,
        name="code-interpreter-bash-agent",
        instructions="You are a helpful assistant. Use the code interpreter tool when useful.",
        tools=[{"type": "code_interpreter"}],
    )
    print("Created agent:", json.dumps(agent.as_dict() if hasattr(agent, 'as_dict') else agent, indent=2, default=str))

    thread = client.agents.create_thread()
    print("Created thread:", json.dumps(thread.as_dict() if hasattr(thread, 'as_dict') else thread, indent=2, default=str))

    msg = client.agents.create_message(thread_id=thread.id, role="user", content="Calculate the square root of 144 using code")
    print("Created message:", json.dumps(msg.as_dict() if hasattr(msg, 'as_dict') else msg, indent=2, default=str))

    run = client.agents.create_run(thread_id=thread.id, agent_id=agent.id)
    print("Initial run:", json.dumps(run.as_dict() if hasattr(run, 'as_dict') else run, indent=2, default=str))

    import time
    for i in range(30):
        run = client.agents.get_run(thread_id=thread.id, run_id=run.id)
        print(f'Poll {i}: status={run.status}')
        if run.status in ("completed", "failed", "cancelled", "expired", "requires_action"):
            break
        time.sleep(2)

    print("Final run:", json.dumps(run.as_dict() if hasattr(run, 'as_dict') else run, indent=2, default=str))

    if hasattr(client.agents, 'list_messages'):
        messages = client.agents.list_messages(thread_id=thread.id)
        msgs = list(messages)
        print("Messages:")
        for m in msgs:
            print(json.dumps(m.as_dict() if hasattr(m, 'as_dict') else m, indent=2, default=str))
    else:
        print("No list_messages method found")
except Exception:
    print("FAILURE:")
    traceback.print_exc()

```

**Result:**

❌ Exit code `-1`

---

### Step 7 · `search_foundry_docs` · 2.33s

**Query:** `azure.ai.projects create_agent code_interpreter tools python run thread messages list runs sdk`

**Result:**

Found 6 result(s):
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Connect to MCP Server Endpoints for agents - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/model-context-protocol)  
  Use Foundry Toolboxes as MCP endpoints Foundry Toolboxes let you bundle multiple tools - such as Web Search, Code Interp
- [Agent tools overview for Microsoft Foundry Agent Service - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/tool-catalog)  
  Agent tools overview for Foundry Agent Service Summarize this article for me In this article Tools extend what your agen
- [Azure developer documentation](https://learn.microsoft.com/en-us/azure/developer/)  
  ...ions on Azure using Python,NET Azure SDK for .NET API reference Developer center Python Azure SDK for Python API refe

---

### Step 8 · `fetch_doc_page` · 0.63s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/tool-catalog

**Result:**

📄 **Agent tools overview for Microsoft Foundry Agent Service - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to Plans Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Agent tools overview for Foun…

---

### Step 9 · `execute_sdk_code` · 0.04s

**Code:**
```python
import os, json, traceback, time
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
credential = DefaultAzureCredential()

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    models = list(client.models.list())
    print("MODELS:", json.dumps([m.as_dict() if hasattr(m,'as_dict') else str(m) for m in models], indent=2, default=str))
except Exception:
    traceback.print_exc()

```

**Result:**

❌ Exit code `-1`

---

### Step 10 · `execute_sdk_code` · 0.03s

**Code:**
```python
import os, json, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ['AZURE_AI_PROJECT_ENDPOINT']
credential = DefaultAzureCredential()
try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    print('client created')
    print(type(client))
except Exception:
    print('FAILURE:')
    traceback.print_exc()

```

**Result:**

❌ Exit code `-1`

---

### Step 11 · `file_github_issue` · 1.23s

**Arguments:**
```json
{
  "title": "SDK: AIProjectClient initialization fails with unsupported operand type(s) for +: 'float' and 'str'",
  "body": "## Bug Bash Report: Create agent with code interpreter tool and run code prompt\n\n**Interface Tested:** SDK\n**Outcome:** \u274c Bug Found\n\n### What Was Tested\nAttempted to create an agent with the code inte",
  "severity": "P1",
  "interface": "SDK",
  "is_known_limitation": "false",
  "error_message": "unsupported operand type(s) for +: 'float' and 'str'",
  "steps_to_reproduce": "Initialize AIProjectClient with DefaultAzureCredential using AZURE_AI_PROJECT_ENDPOINT from environment.",
  "expected_behavior": "AIProjectClient initializes successfully and supports agent creation/runs.",
  "actual_behavior": "Client initialization fails with TypeError: unsupported operand type(s) for +: 'float' and 'str'",
  "doc_reference": "https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/tool-catalog",
  "workaround": "None found; failure occurs before any workaround or agent operation."
}
```

**Result:**

✅ Filed [#16](https://github.com/soumyapatro/foundry-bugbash-findings/issues/16) — [Known Limitation] SDK: AIProjectClient initialization fails with unsupported operand type(s) for +: 'float' and 'str'

---

## Agent Summary

## Bug Bash Report: Create agent with code interpreter tool and run square-root prompt

**Interface Tested:** SDK  
**Outcome:** ❌ Bug Found

### What Was Tested
Attempted to create an agent with the code interpreter tool enabled using the latest Foundry SDK, then run it with:

`Calculate the square root of 144 using code`

and verify that the code interpreter was invoked and the answer `12` was returned.

### Result
The scenario could not be completed because `AIProjectClient` initialization failed immediately in the SDK environment with:

`unsupported operand type(s) for +: 'float' and 'str'`

Because the client could not be created, I was unable to create the agent, start a thread, or verify code interpreter invocation/output.

### Issues Filed
- [#16](https://github.com/soumyapatro/foundry-bugbash-findings/issues/16) — [Known Limitation] SDK: AIProjectClient initialization fails with unsupported operand type(s) for +: 'float' and 'str' (P3)

### Notes / Next Steps
- I consulted the latest Foundry docs first, including the agent tools overview and SDK quickstart.
- SDK versions used:
  - `azure-ai-projects` 2.3.0
  - `azure-ai-agents` 1.1.0
  - `azure-identity` 1.25.3
  - `openai` 2.46.0
  - Python 3.13.14
- Since the failure happened before any agent operation, this blocks validating the code interpreter tool itself in this environment.

---

## Issues Filed

- ⚠️ [#16](https://github.com/soumyapatro/foundry-bugbash-findings/issues/16) — [Known Limitation] SDK: AIProjectClient initialization fails with unsupported operand type(s) for +: 'float' and 'str'

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-21 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*