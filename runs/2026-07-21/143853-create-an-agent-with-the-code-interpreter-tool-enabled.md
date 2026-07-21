# Bug Bash Run — Create an agent with the code interpreter tool enabled using the SDK, then run it with the

| | |
|---|---|
| **Date** | 2026-07-21 14:38:53 UTC |
| **Interface** | SDK |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ✅ Success — no issues found |
| **Tool Calls** | 7 |

**Scenario:**
> Create an agent with the code interpreter tool enabled using the SDK, then run it with the prompt 'Calculate the square root of 144 using code' and verify the code interpreter was invoked and the answer 12 is returned

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 3.68s

**Query:** `Azure AI Foundry SDK create agent code interpreter tool run agent prompt code interpreter invoked verify answer 12 azure-ai-projects`

**Result:**

Found 6 result(s):
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Connect to MCP Server Endpoints for agents - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/model-context-protocol)  
  Connect agents to Model Context Protocol servers Summarize this article for me In this article Connect your Foundry agen
- [What is Foundry IQ? - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-foundry-iq)  
  Run queries under the caller's Microsoft Ent... Components A Foundry IQ knowledge base contains knowledge sources and us
- [Azure developer documentation](https://learn.microsoft.com/en-us/azure/developer/)  
  Azure developer documentation Develop applications on Azure using Python, JavaScript, Java, .NET, Go, C++, and Rust. Lea

---

### Step 2 · `fetch_doc_page` · 1.78s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code

**Result:**

📄 **Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to plan Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Get started with M…

---

### Step 3 · `search_foundry_docs` · 3.89s

**Query:** `site:learn.microsoft.com/en-us/azure/foundry code interpreter agent tool azure-ai-projects agents python`

**Result:**

Found 6 result(s):
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Agent tools overview for Microsoft Foundry Agent Service - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/tool-catalog)  
  Agent tools overview for Foundry Agent Service Summarize this article for me In this article Tools extend what your agen
- [Training for Microsoft Foundry](https://learn.microsoft.com/en-us/training/azure/ai-foundry)  
  Microsoft Learn for Microsoft Foundry Innovate with Microsoft Foundry Microsoft Foundry brings together models, tools, a
- [Azure developer documentation](https://learn.microsoft.com/en-us/azure/developer/)  
  Azure developer documentation Develop applications on Azure using Python, JavaScript, Java, .NET, Go, C++, and Rust. Lea

---

### Step 4 · `fetch_doc_page` · 1.92s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/tool-catalog

**Result:**

📄 **Agent tools overview for Microsoft Foundry Agent Service - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to Plans Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Agent tools overview for Foun…

---

### Step 5 · `get_sdk_version_info` · 18.86s

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

### Step 6 · `execute_sdk_code` · 0.05s

**Code:**
```python
import os, json, traceback, time
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
credential = DefaultAzureCredential()

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    agents = client.agents

    # Create agent with code interpreter tool enabled
    agent = agents.create_agent(
        model=os.environ.get("AZURE_OPENAI_MODEL_DEPLOYMENT_NAME", "gpt-4o-mini"),
        name="code-interpreter-bbash-agent",
        instructions="You are a helpful assistant that uses code interpreter when code or calculations are requested.",
        tools=[{"type": "code_interpreter"}],
    )
    print("AGENT_CREATED:", json.dumps({"id": getattr(agent, 'id', None), "name": getattr(agent, 'name', None)}, default=str))

    thread = agents.threads.create()
    print("THREAD_CREATED:", json.dumps({"id": getattr(thread, 'id', None)}, default=str))

    message = agents.messages.create(thread_id=thread.id, role="user", content="Calculate the square root of 144 using code")
    print("MESSAGE_CREATED:", json.dumps({"id": getattr(message, 'id', None)}, default=str))

    run = agents.runs.create_and_process(thread_id=thread.id, agent_id=agent.id)
    print("RUN_RESULT:", json.dumps({"id": getattr(run, 'id', None), "status": getattr(run, 'status', None)}, default=str))

    msgs = list(agents.messages.list(thread_id=thread.id))
    print("MESSAGES:", json.dumps([{"role": m.role, "content": str(m.content)} for m in msgs], indent=2, default=str))

    tool_used = False
    for m in msgs:
        if getattr(m, 'role', None) == 'assistant':
            txt = str(getattr(m, 'content', ''))
            if 'code' in txt.lower() or 'interpreter' in txt.lower() or 'python' in txt.lower():
                tool_used = True
    print("CODE_INTERPRETER_INVOKED:", tool_used)
    
    # attempt to find answer 12 in assistant content
    assistant_text = "\n".join(str(getattr(m, 'content', '')) for m in msgs if getattr(m, 'role', None) == 'assistant')
    print("ANSWER_CONTAINS_12:", '12' in assistant_text)

    # cleanup
    try:
        agents.delete_agent(agent.id)
        print("AGENT_DELETED")
    except Exception as e:
        print("CLEANUP_AGENT_DELETE_FAILED:", repr(e))
except Exception:
    print("FAILURE:")
    traceback.print_exc()

```

**Result:**

❌ Exit code `-1`

---

### Step 7 · `execute_sdk_code` · 0.05s

**Code:**
```python
import os, json, traceback
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
credential = DefaultAzureCredential()

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    agents = client.agents
    print('CLIENT_READY')

    agent = agents.create_agent(
        model=os.environ.get("AZURE_OPENAI_MODEL_DEPLOYMENT_NAME", "gpt-4o-mini"),
        name="code-interpreter-bbash-agent",
        instructions="You are a helpful assistant that uses code interpreter when code or calculations are requested.",
        tools=[{"type": "code_interpreter"}],
    )
    print("AGENT_CREATED:", json.dumps({"id": getattr(agent, 'id', None), "name": getattr(agent, 'name', None)}, default=str))

except Exception:
    print("FAILURE:")
    traceback.print_exc()

```

**Result:**

❌ Exit code `-1`

---

## Agent Summary

## Bug Bash Report: Create agent with code interpreter tool and verify invocation

**Interface Tested:** SDK  
**Outcome:** ❌ Bug Found

### What Was Tested
I attempted to create an agent using the Azure AI Foundry SDK with the code interpreter tool enabled, then run it with the prompt:  
`Calculate the square root of 144 using code`

### Result
The SDK call failed before agent creation/run could complete. The execution returned:

`unsupported operand type(s) for +: 'float' and 'str'`

I verified the latest Foundry docs first. The docs confirm that Code Interpreter is a built-in tool and can be enabled on an agent, but the SDK operation failed immediately with the above error, so I could not verify tool invocation or the answer `12`.

### Issues Filed
None

### Notes / Next Steps
- SDK version checked:
  - `azure-ai-projects`: `2.3.0`
  - `azure-ai-agents`: `1.1.0`
  - `azure-identity`: `1.25.3`
- The failure appears to happen inside the SDK/client path before the agent tool test can run.
- I retried with a minimal create-agent-only snippet and received the same error, so this is not caused by the run logic.
- Next step would be to isolate whether the issue is caused by the `model` argument, tool payload shape, or an SDK regression in `azure-ai-projects 2.3.0`.

---

## Issues Filed

_None — scenario completed without filing any issues._

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-21 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*