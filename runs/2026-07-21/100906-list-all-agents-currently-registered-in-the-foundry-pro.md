# Bug Bash Run — List all agents currently registered in the Foundry project using the azure-ai-agents SDK 

| | |
|---|---|
| **Date** | 2026-07-21 10:09:06 UTC |
| **Interface** | SDK |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ✅ Success — no issues found |
| **Tool Calls** | 5 |

**Scenario:**
> List all agents currently registered in the Foundry project using the azure-ai-agents SDK and print each agent's name and model

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 2.08s

**Query:** `azure-ai-agents SDK list agents project name model latest Foundry docs`

**Result:**

Found 6 result(s):
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [What is Foundry IQ? - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-foundry-iq)  
  What is Foundry IQ. Summarize this article for me In this article Note Some Foundry IQ features are now generally availa
- [Quickstart: Set up Microsoft Foundry resources - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/tutorials/quickstart-create-foundry-resources)  
  Quickstart: Set up Microsoft Foundry resources Summarize this article for me In this article In this quickstart, you cre
- [Microsoft Agent Framework Agent Types - Microsoft Foundry](https://learn.microsoft.com/en-us/agent-framework/agents/)  
  ...undry Models Azure OpenAI SDK 2 Azure.AI.OpenAI https://ai-foundry-.services.ai.azure.com/ Foundry Models OpenAI SDK 

---

### Step 2 · `fetch_doc_page` · 0.36s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code

**Result:**

📄 **Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to plan Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Get started with M…

---

### Step 3 · `execute_sdk_code` · 0.04s

**Code:**
```python
import os, json, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ['AZURE_AI_PROJECT_ENDPOINT']
credential = DefaultAzureCredential()

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    agents = client.agents.list()
    results = []
    for a in agents:
        name = getattr(a, 'name', None)
        model = getattr(a, 'model', None)
        results.append({'name': name, 'model': model, 'id': getattr(a, 'id', None)})
        print(f"{name} | {model}")
    print('SUCCESS:', json.dumps(results, indent=2, default=str))
except Exception:
    print('FAILURE:')
    traceback.print_exc()
```

**Result:**

❌ Exit code `-1`

---

### Step 4 · `get_sdk_version_info` · 7.28s

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

### Step 5 · `execute_sdk_code` · 0.02s

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
    try:
        agents = client.agents.list()
        print('List object type:', type(agents))
        count = 0
        for a in agents:
            count += 1
            name = getattr(a, 'name', None)
            model = getattr(a, 'model', None)
            print(f'AGENT {count}: name={name!r}, model={model!r}, raw={a!r}')
        print('TOTAL:', count)
    except Exception:
        print('FAILURE in list():')
        traceback.print_exc()
except Exception:
    print('FAILURE creating client:')
    traceback.print_exc()
```

**Result:**

❌ Exit code `-1`

---

## Agent Summary

## Bug Bash Report: List registered agents in Foundry project

**Interface Tested:** SDK  
**Outcome:** ❌ Bug Found

### What Was Tested
Attempted to list all agents currently registered in the active Foundry project using the `azure-ai-agents`/Foundry SDK and print each agent’s `name` and `model`.

### Result
The SDK execution failed before the Python code could run successfully. The execution environment returned:

`unsupported operand type(s) for +: 'float' and 'str'`

I retried with a more defensive script, but the same error occurred. This appears to be an environment/runtime issue in the SDK execution tool rather than a user code error.

### Issues Filed
- None yet

### Notes / Next Steps
- SDK versions captured:
  - `azure-ai-projects`: `2.3.0`
  - `azure-ai-agents`: `1.1.0`
  - `azure-identity`: `1.25.3`
- Because the failure happened inside the execution environment, I could not verify whether agent enumeration itself works against the project.
- If you want, I can next:
  1. try the same operation via REST API, or
  2. file a bug for the SDK execution/runtime error if you’d like me to treat this as a platform issue.

---

## Issues Filed

_None — scenario completed without filing any issues._

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-21 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*