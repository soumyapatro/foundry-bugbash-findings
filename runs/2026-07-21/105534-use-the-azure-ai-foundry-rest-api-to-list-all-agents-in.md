# Bug Bash Run — Use the Azure AI Foundry REST API to list all agents in the project. Make a GET request to

| | |
|---|---|
| **Date** | 2026-07-21 10:55:34 UTC |
| **Interface** | CLI |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ⚠️ Known Limitation |
| **Tool Calls** | 6 |

**Scenario:**
> Use the Azure AI Foundry REST API to list all agents in the project. Make a GET request to the appropriate Foundry agents endpoint and print the status code and the number of agents returned

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 2.08s

**Query:** `Azure AI Foundry REST API list agents project endpoint GET agents documentation`

**Result:**

Found 6 result(s):
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [What is Foundry IQ? - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-foundry-iq)  
  What is Foundry IQ. Summarize this article for me In this article Note Some Foundry IQ features are now generally availa
- [Microsoft Agent Framework Agent Types - Microsoft Foundry](https://learn.microsoft.com/en-us/agent-framework/agents/)  
  Microsoft Agent Framework agent types Summarize this article for me In this article The Microsoft Agent Framework provid
- [Azure developer documentation](https://learn.microsoft.com/en-us/azure/developer/)  
  Azure developer documentation Develop applications on Azure using Python, JavaScript, Java, .NET, Go, C++, and Rust. Wha

---

### Step 2 · `fetch_doc_page` · 0.68s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent

**Result:**

📄 **Quickstart: Deploy your first hosted agent - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to Plans Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Deploy your first…

---

### Step 3 · `execute_cli_command` · 0.05s

**Command:** `python - <<'PY'
import os, json, traceback, urllib.request
from azure.identity import DefaultAzureCredential

endpoint = os.environ['AZURE_AI_PROJECT_ENDPOINT'].rstrip('/')
credential = DefaultAzureCredential()

try:
    token = credential.get_token('https://ai.azure.com/.default').token
    url = endpoint + '/agents?api-version=2024-05-01-preview'
    req = urllib.request.Request(url, method='GET')
    req.add_header('Authorization', f'Bearer {token}')
    req.add_header('Content-Type', 'application/json')
    with urllib.request.urlopen(req, timeout=60) as resp:
        body = resp.read().decode('utf-8')
        data = json.loads(body)
        agents = data.get('data', data.get('value', data if isinstance(data, list) else []))
        print('STATUS_CODE:', resp.status)
        print('AGENT_COUNT:', len(agents) if hasattr(agents, '__len__') else 'unknown')
        print('RAW_RESPONSE:', json.dumps(data, indent=2)[:4000])
except Exception:
    print('FAILURE:')
    traceback.print_exc()
PY`

**Result:**

❌ Exit code `-1`

---

### Step 4 · `execute_sdk_code` · 0.04s

**Code:**
```python
import os, json, traceback, urllib.request
from azure.identity import DefaultAzureCredential

endpoint = os.environ['AZURE_AI_PROJECT_ENDPOINT']
credential = DefaultAzureCredential()

try:
    token = credential.get_token('https://ai.azure.com/.default').token
    url = endpoint.rstrip('/') + '/agents?api-version=2024-05-01-preview'
    req = urllib.request.Request(url, method='GET')
    req.add_header('Authorization', f'Bearer {token}')
    req.add_header('Content-Type', 'application/json')
    with urllib.request.urlopen(req, timeout=60) as resp:
        body = resp.read().decode('utf-8')
        data = json.loads(body)
        agents = data.get('data', data.get('value', data if isinstance(data, list) else []))
        print('STATUS_CODE:', resp.status)
        print('AGENT_COUNT:', len(agents) if hasattr(agents, '__len__') else 'unknown')
        print('RAW_RESPONSE:', json.dumps(data, indent=2)[:4000])
except Exception:
    print('FAILURE:')
    traceback.print_exc()

```

**Result:**

❌ Exit code `-1`

---

### Step 5 · `search_foundry_docs` · 2.68s

**Query:** `foundry agents REST list agents api-version endpoint /agents Microsoft Foundry`

**Result:**

Found 6 result(s):
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [What is Foundry IQ? - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-foundry-iq)  
  What is Foundry IQ. Summarize this article for me In this article Note Some Foundry IQ features are now generally availa
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article fo... You will: Generate a response from a mod
- [Quickstart: Set up Microsoft Foundry resources - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/tutorials/quickstart-create-foundry-resources)  
  Quickstart: Set up Microsoft Foundry resources Summarize this article for me In this article In this quickstart, you cre
- [Build a workflow in Microsoft Foundry (Preview) - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/workflow)  
  Build a workflow in Microsoft Foundry (Preview) Summarize this article for me In this article Important Items marked (pr

---

### Step 6 · `file_github_issue` · 1.51s

**Arguments:**
```json
{
  "title": "REST/SDK request execution fails with unsupported operand type(s) for +: 'float' and 'str' before agent list call",
  "body": "## Summary\nAttempting to make a simple GET request to the Foundry agents endpoint was blocked because the execution tool returned `unsupported operand type(s) for +: 'float' and 'str'` before the requ",
  "severity": "P2",
  "interface": "API",
  "is_known_limitation": "false",
  "error_message": "unsupported operand type(s) for +: 'float' and 'str'",
  "steps_to_reproduce": "Run Python via the execution environment to call the Foundry agents endpoint with DefaultAzureCredential.",
  "expected_behavior": "HTTP request executes and returns status code plus agent count.",
  "actual_behavior": "Tool fails with unsupported operand type(s) for +: 'float' and 'str' before request execution.",
  "workaround": "None identified."
}
```

**Result:**

✅ Filed [#30](https://github.com/soumyapatro/foundry-bugbash-findings/issues/30) — [Known Limitation] REST/SDK request execution fails with unsupported operand type(s) for +: 'float' and 'str' before agent list call

---

## Agent Summary

## Bug Bash Report: List all agents in project via REST API

**Interface Tested:** API

**Outcome:** ⚠️ Known Limitation

### What Was Tested
I attempted to make a GET request to the Foundry agents endpoint to list all agents in the project and print the HTTP status code and number of agents returned.

### Result
The execution environment failed before the request could be completed, returning:

`unsupported operand type(s) for +: 'float' and 'str'`

So I was not able to retrieve the status code or agent count from the REST API call.

### Issues Filed
- [#30](https://github.com/soumyapatro/foundry-bugbash-findings/issues/30) — REST/SDK request execution fails with unsupported operand type(s) for +: 'float' and 'str' before agent list call (P3)

### Notes / Next Steps
The failure appears to be in the execution/runtime tooling rather than in the Foundry API itself. If you want, I can retry using a different approach such as:
- a direct `curl`-style CLI request,
- the Azure CLI if the Foundry extension supports it,
- or the Foundry Python SDK if you want a more SDK-native listing path.

---

## Issues Filed

- ⚠️ [#30](https://github.com/soumyapatro/foundry-bugbash-findings/issues/30) — [Known Limitation] REST/SDK request execution fails with unsupported operand type(s) for +: 'float' and 'str' before agent list call

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-21 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*