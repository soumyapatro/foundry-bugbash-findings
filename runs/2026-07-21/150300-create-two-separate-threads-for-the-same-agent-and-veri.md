# Bug Bash Run — Create two separate threads for the same agent and verify they are independent: send 'reme

| | |
|---|---|
| **Date** | 2026-07-21 15:03:00 UTC |
| **Interface** | SDK |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ❌ Bug Found |
| **Tool Calls** | 9 |

**Scenario:**
> Create two separate threads for the same agent and verify they are independent: send 'remember the word APPLE' on thread 1 and 'remember the word BANANA' on thread 2, then query each thread for what word it was told to remember and confirm they do not bleed into each other

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 2.64s

**Query:** `Azure AI Foundry agents threads memory thread independence remember the word APPLE BANANA azure-ai-projects SDK threads messages documentation`

**Result:**

Found 6 result(s):
- [Data, privacy, and security for Foundry Models sold by Azure in Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy)  
  This article provides details regarding how data provided by you to Foundry Models sold by Azure in Microsoft Foundry ar
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  If you need to create a new Foundry project, you need the Owner role at resource group scope. Install and verify the ext
- [Training for Microsoft Foundry](https://learn.microsoft.com/en-us/training/azure/ai-foundry)  
  Introduction to Microsoft Foundry Explore Microsoft Foundry Your path to getting started with Microsoft Foundry Get star
- [Azure Architecture Center - Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)  
  Popular articles Microsoft Foundry chat baseline architecture Develop and optimize a RAG implementation AI agent orchest

---

### Step 2 · `fetch_doc_page` · 0.97s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent

**Result:**

📄 **Quickstart: Deploy your first hosted agent - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to Plans Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Deploy your first…

---

### Step 3 · `search_foundry_docs` · 3.06s

**Query:** `site:learn.microsoft.com/en-us/azure/foundry threads create message thread SDK azure.ai.projects thread messages agents`

**Result:**

Found 6 result(s):
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Data, privacy, and security for Foundry Models sold by Azure in Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy)  
  This article provides details regarding how data provided by you to Foundry Models sold by Azure in Microsoft Foundry ar
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Training for Microsoft Foundry](https://learn.microsoft.com/en-us/training/azure/ai-foundry)  
  Microsoft Learn for Microsoft Foundry Innovate with Microsoft Foundry Microsoft Foundry brings together models, tools, a
- [Azure Architecture Center - Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)  
  Popular articles Microsoft Foundry chat baseline architecture Develop and optimize a RAG implementation AI agent orchest

---

### Step 4 · `fetch_doc_page` · 0.63s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code

**Result:**

📄 **Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to plan Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Get started with M…

---

### Step 5 · `get_sdk_version_info` · 7.85s

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
import os, json, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ['AZURE_AI_PROJECT_ENDPOINT']
credential = DefaultAzureCredential()

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    agent = client.agents.get_agent(os.environ.get('AZURE_AGENT_ID')) if os.environ.get('AZURE_AGENT_ID') else None
    if agent is None:
        raise RuntimeError('Set AZURE_AGENT_ID to an existing agent id for this test.')
    t1 = client.agents.threads.create()
    t2 = client.agents.threads.create()
    print('THREAD1', t1.id)
    print('THREAD2', t2.id)
    client.agents.messages.create(thread_id=t1.id, role='user', content='remember the word APPLE')
    client.agents.messages.create(thread_id=t2.id, role='user', content='remember the word BANANA')
    r1 = client.agents.runs.create_and_process(thread_id=t1.id, agent_id=agent.id)
    r2 = client.agents.runs.create_and_process(thread_id=t2.id, agent_id=agent.id)
    print('RUN1', r1.status)
    print('RUN2', r2.status)
    client.agents.messages.create(thread_id=t1.id, role='user', content='what word was I told to remember? answer with only the word')
    client.agents.messages.create(thread_id=t2.id, role='user', content='what word was I told to remember? answer with only the word')
    r1b = client.agents.runs.create_and_process(thread_id=t1.id, agent_id=agent.id)
    r2b = client.agents.runs.create_and_process(thread_id=t2.id, agent_id=agent.id)
    msgs1 = list(client.agents.messages.list(thread_id=t1.id))
    msgs2 = list(client.agents.messages.list(thread_id=t2.id))
    print('THREAD1_MESSAGES', json.dumps([getattr(m, 'content', str(m)) for m in msgs1], default=str))
    print('THREAD2_MESSAGES', json.dumps([getattr(m, 'content', str(m)) for m in msgs2], default=str))
    print('SUCCESS')
except Exception:
    print('FAILURE:')
    traceback.print_exc()

```

**Result:**

❌ Exit code `-1`

---

### Step 7 · `execute_sdk_code` · 0.03s

**Code:**
```python
import os, json, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ['AZURE_AI_PROJECT_ENDPOINT']
credential = DefaultAzureCredential()

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    print('CLIENT_OK')
    print('Has agents:', hasattr(client, 'agents'))
    print('Dir agents subset:', [x for x in dir(client.agents) if 'thread' in x.lower() or 'message' in x.lower() or 'run' in x.lower()][:50])
except Exception:
    print('FAILURE:')
    traceback.print_exc()

```

**Result:**

❌ Exit code `-1`

---

### Step 8 · `execute_cli_command` · 0.06s

**Command:** `az --version`

**Result:**

❌ Exit code `-1`

---

### Step 9 · `file_github_issue` · 1.61s

**Arguments:**
```json
{
  "title": "Bug bash blocker: local tooling crashes with TypeError \"unsupported operand type(s) for +: 'float' and 'str'\" before any Foundry SDK/CLI action",
  "body": "## Summary\nAttempting to run any Foundry SDK or CLI test in the bug bash environment fails immediately with `TypeError: unsupported operand type(s) for +: 'float' and 'str'` before any Foundry operati",
  "severity": "P1",
  "interface": "SDK/CLI",
  "error_message": "unsupported operand type(s) for +: 'float' and 'str'",
  "steps_to_reproduce": "Run any execute_sdk_code or execute_cli_command call.",
  "expected_behavior": "SDK/CLI commands should execute normally.",
  "actual_behavior": "Immediate TypeError before any Foundry action runs.",
  "workaround": "None found in this environment. Need environment/tooling fix before Foundry tests can proceed."
}
```

**Result:**

✅ Filed [#36](https://github.com/soumyapatro/foundry-bugbash-findings/issues/36) — [P1] Bug bash blocker: local tooling crashes with TypeError "unsupported operand type(s) for +: 'float' and 'str'" before any Foundry SDK/CLI action

---

## Agent Summary

## Bug Bash Report: Thread independence verification for agent memory

**Interface Tested:** SDK  
**Outcome:** ❌ Bug Found

### What Was Tested
I attempted to create two separate threads for the same agent, send different memory prompts:
- Thread 1: “remember the word APPLE”
- Thread 2: “remember the word BANANA”

Then I planned to query each thread independently to verify the words did not bleed across threads.

### Result
The test could not be completed because the local SDK/CLI tooling failed before any Foundry operation ran. Any invocation of `execute_sdk_code` or `execute_cli_command` immediately returned:

`unsupported operand type(s) for +: 'float' and 'str'`

Because of this blocker, I was unable to verify thread isolation behavior.

### Issues Filed
- [#36](https://github.com/soumyapatro/foundry-bugbash-findings/issues/36) — Bug bash blocker: local tooling crashes with TypeError "unsupported operand type(s) for +: 'float' and 'str'" before any Foundry SDK/CLI action (P1)

### Notes / Next Steps
- SDK versions captured:
  - `azure-ai-projects 2.3.0`
  - `azure-ai-agents 1.1.0`
  - `azure-identity 1.25.3`
  - `openai 2.46.0`
  - `agent-dev-cli 0.0.1b260427`
  - Python `3.13.14`
- Once the tooling issue is resolved, I can rerun the thread-isolation scenario and verify whether thread state stays isolated as expected.

---

## Issues Filed

- ❌ [#36](https://github.com/soumyapatro/foundry-bugbash-findings/issues/36) — [P1] Bug bash blocker: local tooling crashes with TypeError "unsupported operand type(s) for +: 'float' and 'str'" before any Foundry SDK/CLI action

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-21 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*