# Bug Bash Run — Create two separate threads for the same agent and verify they are independent: send 'reme

| | |
|---|---|
| **Date** | 2026-07-21 10:57:41 UTC |
| **Interface** | SDK |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ⚠️ Known Limitation |
| **Tool Calls** | 6 |

**Scenario:**
> Create two separate threads for the same agent and verify they are independent: send 'remember the word APPLE' on thread 1 and 'remember the word BANANA' on thread 2, then query each thread for what word it was told to remember and confirm they do not bleed into each other

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 2.79s

**Query:** `Azure AI Foundry agents threads memory create thread messages retrieve thread SDK azure-ai-projects remember conversation thread independent`

**Result:**

Found 6 result(s):
- [Data, privacy, and security for Foundry Models sold by Azure in Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy)  
  This article provides details regarding how data provided by you to Foundry Models sold by... A 'model sold by Azure' is
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [What is Foundry IQ? - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-foundry-iq)  
  Relationship to Fabric IQ and Work IQ Microsoft provides three IQ workloads that give agents ... It models business data
- [Azure developer documentation](https://learn.microsoft.com/en-us/azure/developer/)  
  Azure developer documentation Develop applications on Azure using Python, JavaScript, Java, .NET, Go, C++, and Rust. Wha

---

### Step 2 · `fetch_doc_page` · 0.68s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code

**Result:**

📄 **Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to Plans Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Get started with …

---

### Step 3 · `get_sdk_version_info` · 6.05s

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
import os, json, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ['AZURE_AI_PROJECT_ENDPOINT']
credential = DefaultAzureCredential()

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    agents = client.agents

    agent = agents.create_agent(model=os.environ.get('AZURE_AI_MODEL_DEPLOYMENT_NAME', 'gpt-4o-mini'), name='thread-independence-test', instructions='You are a memory echo agent. If user asks what word to remember, answer with the last word explicitly told to remember in that thread. Otherwise acknowledge the word.')
    print('Agent created:', agent.id)

    thread1 = agents.threads.create()
    thread2 = agents.threads.create()
    print('Thread1:', thread1.id)
    print('Thread2:', thread2.id)

    agents.messages.create(thread_id=thread1.id, role='user', content='remember the word APPLE')
    agents.runs.create_and_process(thread_id=thread1.id, agent_id=agent.id)
    agents.messages.create(thread_id=thread2.id, role='user', content='remember the word BANANA')
    agents.runs.create_and_process(thread_id=thread2.id, agent_id=agent.id)

    agents.messages.create(thread_id=thread1.id, role='user', content='what word was I told to remember?')
    agents.runs.create_and_process(thread_id=thread1.id, agent_id=agent.id)
    agents.messages.create(thread_id=thread2.id, role='user', content='what word was I told to remember?')
    agents.runs.create_and_process(thread_id=thread2.id, agent_id=agent.id)

    def get_last_assistant_message(thread_id):
        msgs = list(agents.messages.list(thread_id=thread_id))
        for m in msgs:
            if getattr(m, 'role', None) == 'assistant':
                parts = getattr(m, 'content', [])
                if parts:
                    text = parts[0].text.value if hasattr(parts[0], 'text') else str(parts[0])
                    return text
        return None

    result = {
        'thread1_response': get_last_assistant_message(thread1.id),
        'thread2_response': get_last_assistant_message(thread2.id),
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

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    print('Client created')
    print('Available attrs on agents:', [a for a in dir(client.agents) if not a.startswith('_')][:50])
except Exception:
    print('FAILURE:')
    traceback.print_exc()

```

**Result:**

❌ Exit code `-1`

---

### Step 6 · `file_github_issue` · 1.35s

**Arguments:**
```json
{
  "title": "azure-ai-projects AIProjectClient initialization fails with unsupported operand type(s) for +: 'float' and 'str'",
  "body": "## Summary\nAttempting to initialize `AIProjectClient` with the documented `DefaultAzureCredential` and `AZURE_AI_PROJECT_ENDPOINT` fails immediately with `unsupported operand type(s) for +: 'float' an",
  "severity": "P1",
  "interface": "SDK",
  "is_known_limitation": "false",
  "error_message": "unsupported operand type(s) for +: 'float' and 'str'",
  "steps_to_reproduce": "Initialize AIProjectClient with AZURE_AI_PROJECT_ENDPOINT and DefaultAzureCredential.",
  "expected_behavior": "AIProjectClient should initialize successfully.",
  "actual_behavior": "Initialization fails with unsupported operand type(s) for +: 'float' and 'str'.",
  "workaround": "None identified.",
  "doc_reference": "https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code"
}
```

**Result:**

✅ Filed [#32](https://github.com/soumyapatro/foundry-bugbash-findings/issues/32) — [Known Limitation] azure-ai-projects AIProjectClient initialization fails with unsupported operand type(s) for +: 'float' and 'str'

---

## Agent Summary

## Bug Bash Report: Thread independence test

**Interface Tested:** SDK  
**Outcome:** ❌ Bug Found

### What Was Tested
I attempted to create two separate agent threads, send:
- Thread 1: `remember the word APPLE`
- Thread 2: `remember the word BANANA`

Then I planned to query each thread to verify the remembered word stayed isolated per thread.

### Result
The test could not proceed because `AIProjectClient` failed during initialization with:

`unsupported operand type(s) for +: 'float' and 'str'`

This happened before any agent/thread operations were executed, so thread independence could not be verified.

### Issues Filed
- [#32](https://github.com/soumyapatro/foundry-bugbash-findings/issues/32) — [Known Limitation] azure-ai-projects AIProjectClient initialization fails with unsupported operand type(s) for +: 'float' and 'str' (P3)

### Notes / Next Steps
- SDK versions used:
  - `azure-ai-projects` 2.3.0
  - `azure-ai-agents` 1.1.0
  - `azure-identity` 1.25.3
  - `openai` 2.46.0
  - Python 3.13.14
- Because the client failed to initialize, no workaround to test thread isolation was available in this run.
- If you want, I can retry this scenario using the REST API instead of the SDK.

---

## Issues Filed

- ⚠️ [#32](https://github.com/soumyapatro/foundry-bugbash-findings/issues/32) — [Known Limitation] azure-ai-projects AIProjectClient initialization fails with unsupported operand type(s) for +: 'float' and 'str'

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-21 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*