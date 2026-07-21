# Bug Bash Run — Use the Azure AI Foundry REST API to list all agents in the project. Make a GET request to

| | |
|---|---|
| **Date** | 2026-07-21 10:11:40 UTC |
| **Interface** | API |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ✅ Success — no issues found |
| **Tool Calls** | 9 |

**Scenario:**
> Use the Azure AI Foundry REST API to list all agents in the project. Make a GET request to the appropriate Foundry agents endpoint and print the status code and the number of agents returned

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 2.13s

**Query:** `Azure AI Foundry REST API list agents project endpoint GET agents latest docs`

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
- [Data API builder documentation - Data API builder](https://learn.microsoft.com/en-us/azure/data-api-builder/)  
  Data API builder documentation Data API builder generates REST and GraphQL endpoints for your databases, including SQL S

---

### Step 2 · `get_sdk_version_info` · 6.68s

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

### Step 3 · `fetch_doc_page` · 0.39s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent

**Result:**

📄 **Quickstart: Deploy your first hosted agent - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to Plans Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Deploy your first…

---

### Step 4 · `fetch_doc_page` · 0.4s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code

**Result:**

📄 **Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to plan Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Get started with M…

---

### Step 5 · `fetch_doc_page` · 1.07s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code?tabs=rest

**Result:**

📄 **Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to Plans Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Get started with …

---

### Step 6 · `make_api_request` · 0.0s

**Request:** `GET https://ai.azure.com/api/projects`

**Result:**

❌ HTTP `?`

```json
{}
```

---

### Step 7 · `search_foundry_docs` · 2.22s

**Query:** `services.ai.azure.com api projects agents list GET Foundry REST API`

**Result:**

Found 6 result(s):
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [What is Foundry IQ? - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-foundry-iq)  
  What is Foundry IQ. Summarize this article for me In this article Note Some Foundry IQ features are now generally availa
- [Data, privacy, and security for Foundry Models sold by Azure in Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy)  
  ...tails regarding how data provided by you to Foundry Models sold by Azure in Microsoft Foundry are processed, used, an
- [Azure Architecture Center - Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)  
  Popular articles Microsoft Foundry chat baseline architecture Develop and optimize a RAG implementation AI agent orchest
- [Azure developer documentation](https://learn.microsoft.com/en-us/azure/developer/)  
  Azure developer documentation Develop applications on Azure using Python, JavaScript, Java, .NET, Go, C++, and Rust. Wha

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
    # Try to access agents through the projects SDK and print count if possible
    agents_client = client.agents
    print('agents_client_type:', type(agents_client))
    # Common list methods may vary; try several safely
    result = None
    for method_name in ['list', 'list_agents', 'get_all']:
        if hasattr(agents_client, method_name):
            method = getattr(agents_client, method_name)
            try:
                result = method()
                print('SUCCESS_METHOD:', method_name)
                break
            except Exception:
                print(f'METHOD_FAILED: {method_name}')
                traceback.print_exc()
    if result is None:
        print('No list method found on agents client')
    else:
        items = list(result)
        print('STATUS_CODE: N/A (SDK not REST)')
        print('AGENT_COUNT:', len(items))
        print('SAMPLE:', json.dumps([str(x) for x in items[:3]], indent=2))
except Exception:
    print('FAILURE:')
    traceback.print_exc()

```

**Result:**

❌ Exit code `-1`

---

### Step 9 · `make_api_request` · 15.22s

**Request:** `GET https://services.ai.azure.com/api/projects`

**Result:**

❌ HTTP `?`

```json
{}
```

---

## Agent Summary

## Bug Bash Report: List all agents in the project

**Interface Tested:** API

**Outcome:** ❌ Bug Found

### What Was Tested
I attempted to make a GET request to the Azure AI Foundry project API to list agents in the project and report the HTTP status code and returned agent count.

### Result
I was unable to complete the REST API request because the available API request tool rejected valid-looking Foundry service URLs and also errored when passing headers as JSON text.

Observed failures:
- `make_api_request` rejected `https://ai.azure.com/api/projects` with:
  - `URL must target an Azure API domain... Got: https://ai.azure.com/api/projects`
- `make_api_request` then errored on `https://services.ai.azure.com/api/projects` with:
  - `dictionary update sequence element #0 has length 1; 2 is required`

I also attempted a fallback SDK probe, but the SDK execution tool failed before running the code with:
- `unsupported operand type(s) for +: 'float' and 'str'`

Because of these tool-level failures, I could not complete the requested GET request or print a real status code / agent count.

### Issues Filed
None

### Notes / Next Steps
- The likely intended REST endpoint appears to be under `services.ai.azure.com`, but the request tooling is not accepting or correctly parsing the request.
- A follow-up bug bash should verify the API tool’s URL validation and header serialization for Foundry REST requests.
- SDK/CLI versions captured:
  - `azure-ai-projects`: 2.3.0
  - `azure-ai-agents`: 1.1.0
  - `azure-identity`: 1.25.3
  - `agent-dev-cli`: 0.0.1b260427

---

## Issues Filed

_None — scenario completed without filing any issues._

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-21 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*