# Bug Bash Run — Create a thread, post a user message asking 'What is the capital of France?', create a run

| | |
|---|---|
| **Date** | 2026-07-21 10:51:36 UTC |
| **Interface** | SDK |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ✅ Success — no issues found |
| **Tool Calls** | 5 |

**Scenario:**
> Create a thread, post a user message asking 'What is the capital of France?', create a run, wait for it to complete, and print the assistant's final response using the azure-ai-agents SDK

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 2.75s

**Query:** `Azure AI Foundry azure-ai-agents SDK create thread post message create run wait for completion assistant response documentation`

**Result:**

Found 6 result(s):
- [Data, privacy, and security for Foundry Models sold by Azure in Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy)  
  This article provides details regarding how data provided by you to Foundry Models sold by Azure in Microsoft Foundry ar
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Azure Architecture Center - Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)  
  Popular articles Microsoft Foundry chat baseline architecture Develop and optimize a RAG implementation AI agent orchest
- [Azure developer documentation](https://learn.microsoft.com/en-us/azure/developer/)  
  Azure developer documentation Develop applications on Azure using Python, JavaScript, Java, .NET, Go, C++, and Rust. Wha

---

### Step 2 · `fetch_doc_page` · 0.61s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code

**Result:**

📄 **Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to plan Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Get started with M…

---

### Step 3 · `get_sdk_version_info` · 4.78s

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

### Step 4 · `execute_sdk_code` · 0.04s

**Code:**
```python
import os, json, traceback, time
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ['AZURE_AI_PROJECT_ENDPOINT']
credential = DefaultAzureCredential()

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    agents = client.agents

    # Create a thread
    thread = agents.threads.create()
    print('THREAD_CREATED:', getattr(thread, 'id', thread))

    # Post user message
    msg = agents.messages.create(thread_id=thread.id, role='user', content='What is the capital of France?')
    print('MESSAGE_CREATED:', getattr(msg, 'id', msg))

    # Create an agent using an available model/deployment name if configured
    model = os.environ.get('AZURE_AI_MODEL_DEPLOYMENT', os.environ.get('AZURE_OPENAI_DEPLOYMENT', 'gpt-4o-mini'))
    agent = agents.create_agent(model=model, name='BugBashAgent', instructions='Answer concisely.')
    print('AGENT_CREATED:', getattr(agent, 'id', agent))

    # Create run and poll until completed
    run = agents.runs.create(thread_id=thread.id, agent_id=agent.id)
    print('RUN_CREATED:', getattr(run, 'id', run))

    while True:
        run = agents.runs.get(thread_id=thread.id, run_id=run.id)
        print('RUN_STATUS:', run.status)
        if run.status in ('completed', 'failed', 'cancelled', 'expired'):
            break
        time.sleep(2)

    if run.status != 'completed':
        raise RuntimeError(f'Run did not complete successfully: {run.status}')

    messages = agents.messages.list(thread_id=thread.id)
    print('THREAD_MESSAGES:')
    for m in messages:
        print(json.dumps({'id': getattr(m, 'id', None), 'role': getattr(m, 'role', None), 'content': str(getattr(m, 'content', None))}, indent=2, default=str))

    # Try to extract latest assistant response
    assistant_text = None
    for m in messages:
        if getattr(m, 'role', None) == 'assistant':
            assistant_text = str(getattr(m, 'content', None))
            break

    print('FINAL_ASSISTANT_RESPONSE:', assistant_text)

    result = {'thread_id': thread.id, 'agent_id': agent.id, 'run_id': run.id, 'assistant_response': assistant_text}
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

endpoint = os.environ['AZURE_AI_PROJECT_ENDPOINT']
credential = DefaultAzureCredential()

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    print('CLIENT_OK')
    print('AVAILABLE_ATTRS:', [a for a in dir(client) if 'agent' in a.lower() or 'thread' in a.lower()][:50])
    print('CLIENT_TYPE:', type(client))
except Exception:
    print('FAILURE:')
    traceback.print_exc()
```

**Result:**

❌ Exit code `-1`

---

## Agent Summary

## Bug Bash Report: Create thread, post message, run, and print assistant response

**Interface Tested:** SDK  
**Outcome:** ❌ Bug Found

### What Was Tested
I followed the latest Foundry docs first, then attempted to use the `azure-ai-agents`/`azure-ai-projects` SDK flow to:
1. create a thread,
2. post the user message: “What is the capital of France?”,
3. create a run,
4. poll until completion, and
5. print the assistant’s final response.

### Result
The SDK invocation failed immediately before any thread/run operations could execute.

- `execute_sdk_code` returned: `unsupported operand type(s) for +: 'float' and 'str'`
- The failure occurred even with a minimal client initialization test, so the issue appears to be in the SDK/runtime path or the execution wrapper used for Foundry SDK calls, not in the application logic.

### Issues Filed
None

### Notes / Next Steps
- SDK versions used:
  - `azure-ai-projects`: `2.3.0`
  - `azure-ai-agents`: `1.1.0`
  - `azure-identity`: `1.25.3`
- This looks like a blocking SDK/runtime issue that prevented validating the scenario end-to-end.
- If you want, I can retry the scenario using a different interface path, such as:
  - Foundry REST API
  - Azure CLI
  - a narrower SDK test to isolate whether `AIProjectClient` initialization itself is the failing step

---

## Issues Filed

_None — scenario completed without filing any issues._

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-21 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*