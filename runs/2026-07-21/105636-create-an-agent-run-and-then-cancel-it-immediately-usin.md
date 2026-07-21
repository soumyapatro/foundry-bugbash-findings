# Bug Bash Run — Create an agent run and then cancel it immediately using the SDK before it completes. Veri

| | |
|---|---|
| **Date** | 2026-07-21 10:56:36 UTC |
| **Interface** | SDK |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ❌ Bug Found |
| **Tool Calls** | 8 |

**Scenario:**
> Create an agent run and then cancel it immediately using the SDK before it completes. Verify the run status transitions to 'cancelled'

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 2.55s

**Query:** `Azure AI Foundry SDK agent run cancel run status cancelled azure-ai-projects docs`

**Result:**

Found 6 result(s):
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [What is Foundry IQ? - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-foundry-iq)  
  What is Foundry IQ. Summarize this article for me In this article Note Some Foundry IQ features are now generally availa
- [Quickstart: Set up Microsoft Foundry resources - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/tutorials/quickstart-create-foundry-resources)  
  Quickstart: Set up Microsoft Foundry resources Summarize this article for me In this article In this quickstart, you cre
- [Azure Architecture Center - Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)  
  Popular articles Microsoft Foundry chat baseline architecture Develop and optimize a RAG implementation AI agent orchest

---

### Step 2 · `fetch_doc_page` · 0.75s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code

**Result:**

📄 **Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to plan Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Get started with M…

---

### Step 3 · `search_foundry_docs` · 3.05s

**Query:** `site:learn.microsoft.com/azure/foundry cancel agent run azure-ai-projects cancel_run`

**Result:**

Found 6 result(s):
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Data, privacy, and security for Foundry Models sold by Azure in Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy)  
  This article provides details regarding how data provided by you to Foundry Models sold by Azure in Microsoft Foundry ar
- [What is Foundry IQ? - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-foundry-iq)  
  What is Foundry IQ. Summarize this article for me In this article Note Some Foundry IQ features are now generally availa
- [Training for Microsoft Foundry](https://learn.microsoft.com/en-us/training/azure/ai-foundry)  
  Microsoft Learn for Microsoft Foundry Innovate with Microsoft Foundry Microsoft Foundry brings together models, tools, a

---

### Step 4 · `execute_sdk_code` · 0.04s

**Code:**
```python
import os, json, traceback, time
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
credential = DefaultAzureCredential()

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    print('Client created')
    agent = client.agents.create_agent(
        model=os.environ.get('AZURE_AI_MODEL_DEPLOYMENT_NAME', 'gpt-4o-mini'),
        name='cancel-test-agent',
        instructions='You are a helpful assistant.'
    )
    print('AGENT_CREATED', agent.id)
    thread = client.agents.create_thread()
    print('THREAD_CREATED', thread.id)
    run = client.agents.create_run(thread_id=thread.id, agent_id=agent.id)
    print('RUN_CREATED', run.id, 'status=', getattr(run, 'status', None))
    cancelled = client.agents.cancel_run(thread_id=thread.id, run_id=run.id)
    print('CANCEL_RESPONSE', cancelled.id, 'status=', getattr(cancelled, 'status', None))
    final = client.agents.get_run(thread_id=thread.id, run_id=run.id)
    print('FINAL_STATUS', final.status)
    result = {'run_id': run.id, 'final_status': final.status}
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
import os, json, traceback, time
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
credential = DefaultAzureCredential()

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    print('CLIENT_OK')
    print('SDK_VERSION_OK')
    print('Available agents methods:', [m for m in dir(client.agents) if 'run' in m.lower() or 'thread' in m.lower() or 'agent' in m.lower()][:50])
    
except Exception:
    print('FAILURE:')
    traceback.print_exc()

```

**Result:**

❌ Exit code `-1`

---

### Step 6 · `get_sdk_version_info` · 4.85s

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

### Step 7 · `search_foundry_docs` · 2.43s

**Query:** `azure-ai-projects 2.3.0 unsupported operand type(s) for +: 'float' and 'str'`

**Result:**

Found 6 result(s):
- [Build a workflow in Microsoft Foundry (Preview) - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/workflow)  
  Build a workflow in Microsoft Foundry (Preview) Summarize this article for me In this article Important Items marked (pr
- [How to generate embeddings with Azure OpenAI in Microsoft Foundry Models - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/embeddings)  
  Learn how to generate embeddings Summarize this article for me In this article An embedding is a special format of data 
- [What's New in SQL Server 2025 - SQL Server](https://learn.microsoft.com/en-us/sql/sql-server/what-s-new-in-sql-server-2025)  
  What's new in SQL Server 2025 Summarize this article for me In this article Applies to: SQL Server 2025 (17.x) SQL Serve
- [int, bigint, smallint, and tinyint (Transact-SQL) - SQL Server](https://learn.microsoft.com/en-us/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql)  
  int, bigint, smallint, and tinyint (Transact-SQL) Summarize this article for me In this article Applies to: SQL Server A
- [Evaluation](https://learn.microsoft.com/en-us/agent-framework/agents/evaluation)  
  Evaluation Summarize this article for me In this article Agent Framework includes a built-in evaluation framework that l

---

### Step 8 · `file_github_issue` · 1.25s

**Arguments:**
```json
{
  "title": "azure-ai-projects: AIProjectClient initialization fails with TypeError unsupported operand type(s) for +: 'float' and 'str'",
  "body": "## Summary\nInstantiating `AIProjectClient` from `azure-ai-projects` 2.3.0 fails immediately with `TypeError: unsupported operand type(s) for +: 'float' and 'str'`, preventing any agent run create/canc",
  "severity": "P1",
  "interface": "SDK",
  "error_message": "unsupported operand type(s) for +: 'float' and 'str'",
  "steps_to_reproduce": "Instantiate AIProjectClient with a valid AZURE_AI_PROJECT_ENDPOINT and DefaultAzureCredential using azure-ai-projects 2.3.0 on Python 3.13.14.",
  "expected_behavior": "AIProjectClient should initialize successfully.",
  "actual_behavior": "Initialization fails with TypeError unsupported operand type(s) for +: 'float' and 'str'.",
  "doc_reference": "https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code",
  "workaround": "None found."
}
```

**Result:**

✅ Filed [#31](https://github.com/soumyapatro/foundry-bugbash-findings/issues/31) — [P1] azure-ai-projects: AIProjectClient initialization fails with TypeError unsupported operand type(s) for +: 'float' and 'str'

---

## Agent Summary

## Bug Bash Report: Cancel agent run immediately after creation

**Interface Tested:** SDK  
**Outcome:** ❌ Bug Found

### What Was Tested
I attempted to create an agent run using the Azure AI Foundry Python SDK, cancel it immediately, and then verify that the run status transitions to `cancelled`.

### Result
The test could not proceed because `AIProjectClient` failed during initialization with:

```text
unsupported operand type(s) for +: 'float' and 'str'
```

This happened before any run creation or cancellation logic could execute, so I was not able to verify the `cancelled` status transition.

### Issues Filed
- [#31](https://github.com/soumyapatro/foundry-bugbash-findings/issues/31) — azure-ai-projects: AIProjectClient initialization fails with TypeError unsupported operand type(s) for +: 'float' and 'str' (P1)

### Notes / Next Steps
- The failure appears to block all SDK-based agent scenarios in this environment.
- I consulted the latest Foundry docs before testing.
- SDK versions captured:
  - `azure-ai-projects`: 2.3.0
  - `azure-ai-agents`: 1.1.0
  - `azure-identity`: 1.25.3
  - Python: 3.13.14

---

## Issues Filed

- ❌ [#31](https://github.com/soumyapatro/foundry-bugbash-findings/issues/31) — [P1] azure-ai-projects: AIProjectClient initialization fails with TypeError unsupported operand type(s) for +: 'float' and 'str'

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-21 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*