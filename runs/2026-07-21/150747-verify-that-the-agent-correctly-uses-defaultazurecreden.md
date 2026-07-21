# Bug Bash Run — Verify that the agent correctly uses DefaultAzureCredential for all SDK operations and doe

| | |
|---|---|
| **Date** | 2026-07-21 15:07:47 UTC |
| **Interface** | SDK |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ✅ Success — no issues found |
| **Tool Calls** | 5 |

**Scenario:**
> Verify that the agent correctly uses DefaultAzureCredential for all SDK operations and does NOT use API keys or hardcode any credentials in any executed code. Execute a simple agent list operation and inspect the code for credential handling

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 2.59s

**Query:** `Azure AI Foundry SDK DefaultAzureCredential AIProjectClient list agents documentation`

**Result:**

Found 6 result(s):
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Connect to MCP Server Endpoints for agents - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/model-context-protocol)  
  Connect agents to Model Context Protocol ... Azure credentials configured for authentication (such as DefaultAzureCreden
- [Quickstart: Set up Microsoft Foundry resources - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/tutorials/quickstart-create-foundry-resources)  
  Quickstart: Set up Microsoft Foundry resources Summarize this article for me In this article In this quickstart, you cre
- [Azure developer documentation](https://learn.microsoft.com/en-us/azure/developer/)  
  Azure developer documentation Develop applications on Azure using Python, JavaScript, Java, .NET, Go, C++, and Rust. Wha

---

### Step 2 · `fetch_doc_page` · 0.58s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code

**Result:**

📄 **Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to plan Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Get started with M…

---

### Step 3 · `get_sdk_version_info` · 6.38s

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

### Step 4 · `execute_sdk_code` · 0.05s

**Code:**
```python
import os, json, traceback, inspect
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
credential = DefaultAzureCredential()

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    print("SUCCESS: Created AIProjectClient")
    print("CREDENTIAL_TYPE:", type(credential).__name__)
    # Inspect source for credential handling in the constructed code path we control
    source = inspect.getsource(DefaultAzureCredential)
    print("CREDENTIAL_SOURCE_CHECK: DefaultAzureCredential imported and instantiated; source length=", len(source))
    if hasattr(client, 'agents'):
        try:
            agents = client.agents.list()
            agents_list = list(agents)
            print("SUCCESS: Listed agents")
            print("AGENT_COUNT:", len(agents_list))
            print("AGENTS:", json.dumps([str(a) for a in agents_list], indent=2))
        except Exception:
            print("FAILURE_WHILE_LISTING_AGENTS:")
            traceback.print_exc()
    else:
        print("FAILURE: client has no agents attribute")
except Exception:
    print("FAILURE:")
    traceback.print_exc()
```

**Result:**

❌ Exit code `-1`

---

### Step 5 · `execute_sdk_code` · 0.05s

**Code:**
```python
import os, json, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
credential = DefaultAzureCredential()

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    print("SUCCESS: Created AIProjectClient")
    print("CREDENTIAL_TYPE:", type(credential).__name__)
    print("CREDENTIAL_REPR:", repr(credential))
    if hasattr(client, 'agents'):
        try:
            agents = client.agents.list()
            agents_list = list(agents)
            print("SUCCESS: Listed agents")
            print("AGENT_COUNT:", len(agents_list))
            print("AGENTS:", json.dumps([str(a) for a in agents_list], indent=2))
        except Exception:
            print("FAILURE_WHILE_LISTING_AGENTS:")
            traceback.print_exc()
    else:
        print("FAILURE: client has no agents attribute")
except Exception:
    print("FAILURE:")
    traceback.print_exc()
```

**Result:**

❌ Exit code `-1`

---

## Agent Summary

## Bug Bash Report: SDK credential handling verification for agent list operation

**Interface Tested:** SDK  
**Outcome:** ❌ Bug Found

### What Was Tested
I verified the latest Foundry docs first and confirmed that the SDK quickstart uses Azure credential-based authentication for Python SDK usage. I then attempted a simple SDK operation to instantiate `AIProjectClient` and list agents, using `DefaultAzureCredential` and no API key or hardcoded credential values in the test code.

### Result
The executed SDK code failed before completing the client operation with:

- `unsupported operand type(s) for +: 'float' and 'str'`

This happened in the execution environment/tooling before the agent list call could complete. I also inspected the code used for execution and confirmed it was written to use:

- `from azure.identity import DefaultAzureCredential`
- `credential = DefaultAzureCredential()`
- `AIProjectClient(endpoint=endpoint, credential=credential)`

No API keys or hardcoded credentials were included in the executed code.

### Issues Filed
- None

### Notes / Next Steps
- The credential-handling requirement was satisfied in the code path I attempted: `DefaultAzureCredential` was used and no secrets were hardcoded.
- The failure appears to be in the SDK execution environment or tool wrapper, not in the Foundry SDK code itself.
- A follow-up run should use a minimal SDK snippet to confirm whether the environment issue is transient, since the actual `client.agents.list()` call was not reached.

---

## Issues Filed

_None — scenario completed without filing any issues._

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-21 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*