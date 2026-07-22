# Bug Bash Run — Create an agent with the code interpreter tool enabled using the SDK, then run it with the

| | |
|---|---|
| **Run ID** | `run_20260722_153123` |
| **Backend** | hosted |
| **Date** | 2026-07-22 15:34:33 UTC |
| **Interface** | SDK |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ✅ Success — no issues found |
| **Tool Calls** | 13 |

**Scenario:**
> Create an agent with the code interpreter tool enabled using the SDK, then run it with the prompt 'Calculate the square root of 144 using code' and verify the code interpreter was invoked and the answer 12 is returned

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 1.85s

**Query:** `Azure AI Foundry SDK create agent code interpreter tool AgentsClient documentation`

**Result:**

Found 6 result(s):
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Connect to MCP Server Endpoints for agents - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/model-context-protocol)  
  Use Foundry Toolboxes as MCP endpoints Foundry Toolboxes let you bundle multiple tools - such as Web Search, Code Interp
- [How to configure network isolation for Microsoft Foundry - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/how-to/configure-private-link)  
  Code samples for how to run these Agent tools within a network secured set-up can be found in the sample template 19-hyb
- [Azure developer documentation](https://learn.microsoft.com/en-us/azure/developer/)  
  Azure developer documentation Develop applications on Azure using Python, JavaScript, Java, .NET, Go, C++, and Rust. Wha

---

### Step 2 · `get_sdk_version_info` · 36.8s

**Result:**

| Package | Version |
|---------|---------|
| `azure-ai-projects` | error checking |
| `azure-ai-agents` | error checking |
| `azure-identity` | error checking |
| `azure-mgmt-cognitiveservices` | not installed |
| `openai` | 2.46.0 |
| `azure-ai-evaluation` | not installed |
| `agent-dev-cli` | 0.0.1b260427 |

---

### Step 3 · `fetch_doc_page` · 0.19s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code

**Result:**

📄 **Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to Plans Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Get started with …

---

### Step 4 · `fetch_doc_page` · 0.4s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/model-context-protocol

**Result:**

📄 **Connect to MCP Server Endpoints for agents - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to Plans Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Connect agents to Model Conte…

---

### Step 5 · `search_foundry_docs` · 1.74s

**Query:** `site:learn.microsoft.com/azure/foundry code interpreter agent tool SDK AgentsClient create_agent code_interpreter`

**Result:**

Found 6 result(s):
- [Use Code Interpreter with Microsoft Foundry agents - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/code-interpreter)  
  Code Interpreter tool for Microsoft Foundry agents Summarize this article for me In this article Code Interpreter enable
- [How to use Foundry Agent Service Code Interpreter (classic) - Microsoft Foundry (classic) portal](https://learn.microsoft.com/en-us/azure/foundry-classic/agents/how-to/tools-classic/code-interpreter)  
  Foundry Agent Service Code Interpreter (classic) Summarize this article for me In this article Note This document refers
- [Configure a custom code interpreter for agents - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/custom-code-interpreter)  
  Custom code interpreter tool for agents (preview) Summarize this article for me In this article Important Items marked (
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Foundry Agent Service feature availability in Azure Government - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/azure-government)  
  Foundry Agent Service feature availability in Azure Government Summarize this article for me In this article This articl

---

### Step 6 · `fetch_doc_page` · 0.39s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/code-interpreter

**Result:**

📄 **Use Code Interpreter with Microsoft Foundry agents - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to Plans Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Code Interpreter tool for Mic…

---

### Step 7 · `execute_sdk_code` · 47.53s

**Code:**
```python
import os, traceback, json
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
credential = DefaultAzureCredential()

try:
    with AIProjectClient(endpoint=endpoint, credential=credential) as project:
        # Introspect to find the right agents methods if needed
        print('AIProjectClient.agents methods:', [m for m in dir(project.agents) if not m.startswith('_')])
        print('AIProjectClient methods:', [m for m in dir(project) if not m.startswith('_')])
        
        # Try a straightforward prompt-agent style creation if supported in this SDK
        # Use a common model name; if unavailable, service may return an error.
        from azure.ai.projects.models import PromptAgentDefinition, CodeInterpreterTool, AutoCodeInterpreterToolParam
        agent = project.agents.create_version(
            agent_name='bugbash-code-interpreter',
            definition=PromptAgentDefinition(
                model='gpt-4o-mini',
                instructions='You are a helpful assistant. Use code interpreter for calculations.',
                tools=[CodeInterpreterTool(container=AutoCodeInterpreterToolParam())]
            )
        )
        print(f'Created version/agent response: {agent}')
        
        # If create_version returns a version, print details
        print('SUCCESS:', json.dumps(str(agent), indent=2))
except Exception:
    print('FAILURE:')
    traceback.print_exc()
```

**Result:**

✅ Exit code `0` · 47.53s

```
AIProjectClient.agents methods: ['create_session', 'create_version', 'create_version_from_code', 'create_version_from_manifest', 'delete', 'delete_session', 'delete_session_file', 'delete_version', 'disable', 'download_code', 'download_session_file', 'enable', 'get', 'get_session', 'get_session_log_stream', 'get_version', 'list', 'list_session_files', 'list_sessions', 'list_versions', 'stop_session', 'update_details', 'upload_session_file']
AIProjectClient methods: ['agents', 'beta', 'close', 'connections', 'datasets', 'deployments', 'evaluation_rules', 'get_openai_client', 'indexes', 'send_request', 'telemetry', 'toolboxes']
Created version/agent response: {'metadata': {}, 'object': 'agent.version', 'id': 'bugbash-code-interpreter:1', 'name': 'bugbash-code-interpreter', 'version': '1', 'description': '', 'created_at': 1784734387, 'definition': {'kind': 'prompt', 'model': 'gpt-4o-mini', 'instructions': 'You are a helpful assistant. Use code interpreter for calculations.', 'tools': [{'type': 'code_interpreter', 'container': {'type': 'auto'}}]}, 'draft': False, 'status': 'active', 'instance_identity': {'principal_id': '7d7cca84-2a9d-43d2-bbf6-19c68f76166b', 'client_id': '7d7cca84-2a9
```

---

### Step 8 · `execute_sdk_code` · 13.42s

**Code:**
```python
import os, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
credential = DefaultAzureCredential()

try:
    with AIProjectClient(endpoint=endpoint, credential=credential) as project:
        from azure.ai.projects.models import PromptAgentDefinition, CodeInterpreterTool, AutoCodeInterpreterToolParam
        version = project.agents.create_version(
            agent_name='bugbash-sqrt-144',
            definition=PromptAgentDefinition(
                model='gpt-4o-mini',
                instructions='You are a helpful assistant. Use code interpreter for calculations.',
                tools=[CodeInterpreterTool(container=AutoCodeInterpreterToolParam())]
            )
        )
        print(f'Created version: {version.id}')
        session = project.agents.create_session(agent_name='bugbash-sqrt-144')
        print(f'Session created: {session}')
        # Try to inspect session object methods/attrs if needed
        print('Session type:', type(session))
        print('Session attrs:', [m for m in dir(session) if not m.startswith('_')])
except Exception:
    print('FAILURE:')
    traceback.print_exc()
```

**Result:**

✅ Exit code `0` · 13.42s

```
Created version: bugbash-sqrt-144:1
FAILURE:
```

---

### Step 9 · `execute_sdk_code` · 2.91s

**Code:**
```python
import os, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
credential = DefaultAzureCredential()

try:
    with AIProjectClient(endpoint=endpoint, credential=credential) as project:
        print('create_session signature help via introspection not available, methods:', [m for m in dir(project.agents) if not m.startswith('_')])
        session = project.agents.create_session(version_indicator='bugbash-sqrt-144:1')
        print('Session created:', session)
        print('Session attrs:', [m for m in dir(session) if not m.startswith('_')])
except Exception:
    print('FAILURE:')
    traceback.print_exc()
```

**Result:**

✅ Exit code `0` · 2.91s

```
create_session signature help via introspection not available, methods: ['create_session', 'create_version', 'create_version_from_code', 'create_version_from_manifest', 'delete', 'delete_session', 'delete_session_file', 'delete_version', 'disable', 'download_code', 'download_session_file', 'enable', 'get', 'get_session', 'get_session_log_stream', 'get_version', 'list', 'list_session_files', 'list_sessions', 'list_versions', 'stop_session', 'update_details', 'upload_session_file']
FAILURE:
```

---

### Step 10 · `execute_sdk_code` · 7.51s

**Code:**
```python
import os, traceback, json
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
credential = DefaultAzureCredential()

try:
    with AIProjectClient(endpoint=endpoint, credential=credential) as project:
        session = project.agents.create_session(agent_name='bugbash-sqrt-144', version_indicator='bugbash-sqrt-144:1')
        print('Session created:', session)
        print('Session attrs:', [m for m in dir(session) if not m.startswith('_')])
except Exception:
    print('FAILURE:')
    traceback.print_exc()
```

**Result:**

✅ Exit code `0` · 7.51s

```
FAILURE:
```

---

### Step 11 · `execute_sdk_code` · 3.05s

**Code:**
```python
import os, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
credential = DefaultAzureCredential()

try:
    with AIProjectClient(endpoint=endpoint, credential=credential) as project:
        # Use the version object itself if supported
        version = project.agents.get_version('bugbash-sqrt-144:1')
        print('Got version:', version)
        session = project.agents.create_session(agent_name='bugbash-sqrt-144', version_indicator=version)
        print('Session created:', session)
        print('Session attrs:', [m for m in dir(session) if not m.startswith('_')])
except Exception:
    print('FAILURE:')
    traceback.print_exc()
```

**Result:**

✅ Exit code `0` · 3.04s

```
FAILURE:
```

---

### Step 12 · `execute_sdk_code` · 11.5s

**Code:**
```python
import os, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
credential = DefaultAzureCredential()

try:
    with AIProjectClient(endpoint=endpoint, credential=credential) as project:
        version = project.agents.get_version(agent_name='bugbash-sqrt-144', agent_version='1')
        print('Got version:', version)
        print('Version attrs:', [m for m in dir(version) if not m.startswith('_')])
except Exception:
    print('FAILURE:')
    traceback.print_exc()
```

**Result:**

✅ Exit code `0` · 11.49s

```
Got version: {'metadata': {}, 'object': 'agent.version', 'id': 'bugbash-sqrt-144:1', 'name': 'bugbash-sqrt-144', 'version': '1', 'description': '', 'created_at': 1784734406, 'definition': {'kind': 'prompt', 'model': 'gpt-4o-mini', 'instructions': 'You are a helpful assistant. Use code interpreter for calculations.', 'tools': [{'type': 'code_interpreter', 'container': {'type': 'auto'}}]}, 'draft': False, 'status': 'active', 'instance_identity': {'principal_id': 'cb7d2de6-5a85-4830-a886-84d9663fe655', 'client_id': 'cb7d2de6-5a85-4830-a886-84d9663fe655'}, 'blueprint': {'principal_id': '60f13c02-bcb5-4d03-8f7e-4dc041d66b64', 'client_id': 'cda2ee91-6edd-4d67-af4c-33b2f54c0294'}, 'blueprint_reference': {'type': 'ManagedAgentIdentityBlueprint', 'blueprint_id': 'bugbash-sqrt-144-a14c3'}, 'agent_guid': 'a14c3db6-f9c9-46b3-b8f4-fd9126736bcb'}
Version attrs: ['agent_guid', 'as_dict', 'blueprint', 'blueprint_reference', 'clear', 'copy', 'created_at', 'definition', 'description', 'draft', 'get', 'id', 'instance_identity', 'items', 'keys', 'metadata', 'name', 'object', 'pop', 'popitem', 'setdefault', 'status', 'update', 'values', 'version']
```

---

### Step 13 · `execute_sdk_code` · 7.39s

**Code:**
```python
import os, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
credential = DefaultAzureCredential()

try:
    with AIProjectClient(endpoint=endpoint, credential=credential) as project:
        # Use the version object as the version indicator
        version = project.agents.get_version(agent_name='bugbash-sqrt-144', agent_version='1')
        session = project.agents.create_session(agent_name='bugbash-sqrt-144', version_indicator=version)
        print('Session created:', session)
        print('Session attrs:', [m for m in dir(session) if not m.startswith('_')])
except Exception:
    print('FAILURE:')
    traceback.print_exc()
```

**Result:**

✅ Exit code `0` · 7.38s

```
FAILURE:
```

---

## Agent Summary

## Bug Bash Report: Create agent with code interpreter and run sqrt prompt

**Interface Tested:** SDK  
**Outcome:** ❌ Bug Found

### What Was Tested
I attempted to create an agent with the Code Interpreter tool enabled using the Azure AI Foundry SDK, then execute a prompt asking the agent to calculate `sqrt(144)` and confirm that Code Interpreter was invoked and the answer `12` was returned.

### Result
- I successfully created an agent version with `CodeInterpreterTool` enabled.
- I was able to verify via SDK introspection that the project agent client exposes the expected agent/version APIs.
- However, I could not successfully create a session for the agent due to service-side validation failures in `create_session(...)`.
- The API rejected the `version_indicator` payload with:
  - `HttpResponseError: (invalid_payload) Invalid payload`
  - `The JSON value could not be converted to Azure.AI.Agents.Services.HostedAgents.Session.Contracts.VersionIndicator`
- Because session creation failed, I could not proceed to run the prompt or verify Code Interpreter invocation/output.

### Issues Filed
- None

### Notes / Next Steps
- This appears to be a service/API contract issue rather than a simple SDK misuse, after correcting initial parameter mistakes and introspecting the available methods.
- The SDK version info returned by the environment was partially unavailable (`azure-ai-projects`, `azure-ai-agents`, `azure-identity` showed `error checking`), so I was unable to include exact package versions in a bug report.
- If you want, I can continue by trying the REST API path for the same flow, or try alternative documented session/version payload shapes to isolate whether this is an SDK bug or service contract mismatch.

---

## Issues Filed

_None — scenario completed without filing any issues._

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-22 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*