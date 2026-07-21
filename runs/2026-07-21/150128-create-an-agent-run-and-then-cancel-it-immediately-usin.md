# Bug Bash Run — Create an agent run and then cancel it immediately using the SDK before it completes. Veri

| | |
|---|---|
| **Date** | 2026-07-21 15:01:28 UTC |
| **Interface** | SDK |
| **Agent Model** | `gpt-5-4-mini` |
| **Outcome** | ⚠️ Known Limitation |
| **Tool Calls** | 6 |

**Scenario:**
> Create an agent run and then cancel it immediately using the SDK before it completes. Verify the run status transitions to 'cancelled'

---

## Execution Trace

### Step 1 · `search_foundry_docs` · 6.19s

**Query:** `Azure AI Foundry SDK create agent run cancel run status cancelled azure-ai-projects documentation`

**Result:**

Found 6 result(s):
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Quickstart: Set up Microsoft Foundry resources - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/tutorials/quickstart-create-foundry-resources)  
  Quickstart: Set up Microsoft Foundry resources Summarize this article for me In this article In this quickstart, you cre
- [Azure Architecture Center - Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)  
  Popular articles Microsoft Foundry chat baseline architecture Develop and optimize a RAG implementation AI agent orchest
- [Azure developer documentation](https://learn.microsoft.com/en-us/azure/developer/)  
  Azure developer documentation Develop applications on Azure using Python, JavaScript, Java, .NET, Go, C++, and Rust. Wha

---

### Step 2 · `fetch_doc_page` · 1.5s

**URL:** https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code

**Result:**

📄 **Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry | Microsoft Learn**

> Table of contents Exit editor mode Ask Learn Ask Learn Reading mode Table of contents Read in English Add Add to plan Edit Copy Markdown Print Note Access to this page requires authorization. You can try signing in or changing directories . Access to this page requires authorization. You can try changing directories . Quickstart: Get started with M…

---

### Step 3 · `search_foundry_docs` · 2.41s

**Query:** `site:learn.microsoft.com azure-ai-projects agent run cancel cancel_run run status cancelled`

**Result:**

Found 6 result(s):
- [Quickstart: Deploy your first hosted agent - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)  
  Quickstart: Deploy your first hosted agent Summarize this article for me In this article Prerequisites Before you begin,
- [Quickstart: Get started with Microsoft Foundry SDK - Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code)  
  Quickstart: Get started with Microsoft Foundry SDK Summarize this article for me In this article In this quickstart you'
- [Training for Microsoft Foundry](https://learn.microsoft.com/en-us/training/azure/ai-foundry)  
  Microsoft Learn for Microsoft Foundry Innovate with Microsoft Foundry Microsoft Foundry brings together models, tools, a
- [Azure Architecture Center - Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)  
  Popular articles Microsoft Foundry chat baseline architecture Develop and optimize a RAG implementation AI agent orchest
- [Plan and Prepare to Develop AI Solutions on Azure - Training](https://learn.microsoft.com/en-us/training/modules/prepare-azure-ai-development/)  
  Learning objectives By the end of this module, you'll be able to: Identify common AI capabilities that you can implement

---

### Step 4 · `execute_sdk_code` · 0.03s

**Code:**
```python
import os, json, traceback, time
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

endpoint = os.environ["AZURE_AI_PROJECT_ENDPOINT"]
subscription = os.environ.get("AZURE_SUBSCRIPTION_ID", "")
credential = DefaultAzureCredential()

try:
    client = AIProjectClient(endpoint=endpoint, credential=credential)
    agents = client.agents
    model = os.environ.get("AZURE_OPENAI_MODEL", "gpt-4o-mini")
    agent = agents.create_agent(model=model, name="cancel-run-test-agent", instructions="You are a helpful assistant.")
    print("AGENT_CREATED:", agent.id)
    thread = agents.threads.create()
    print("THREAD_CREATED:", thread.id)
    msg = agents.messages.create(thread_id=thread.id, role="user", content="Write a long explanation about the solar system.")
    print("MESSAGE_CREATED:", msg.id)
    run = agents.runs.create(thread_id=thread.id, agent_id=agent.id)
    print("RUN_CREATED:", run.id, "STATUS:", run.status)
    cancelled = agents.runs.cancel(thread_id=thread.id, run_id=run.id)
    print("CANCEL_REQUESTED:", cancelled.id, "STATUS:", cancelled.status)
    for i in range(20):
        current = agents.runs.get(thread_id=thread.id, run_id=run.id)
        print(f"POLL_{i}: status={current.status}")
        if str(current.status).lower() == "cancelled":
            print("FINAL_STATUS: cancelled")
            break
        time.sleep(1)
    else:
        print("FINAL_STATUS_NOT_CANCELLED:", current.status)
    agents.delete_agent(agent.id)
except Exception:
    print("FAILURE:")
    traceback.print_exc()
```

**Result:**

❌ Exit code `-1`

---

### Step 5 · `get_sdk_version_info` · 6.1s

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

### Step 6 · `file_github_issue` · 1.27s

**Arguments:**
```json
{
  "title": "SDK execute_sdk_code fails with unsupported operand type(s) for +: 'float' and 'str'",
  "body": "## Summary\n`execute_sdk_code` failed before running user Python with the error `unsupported operand type(s) for +: 'float' and 'str'`.\n\n## Interface\nSDK test execution via bug bash tool (`execute_sdk_",
  "severity": "P1",
  "interface": "SDK",
  "is_known_limitation": "false",
  "error_message": "unsupported operand type(s) for +: 'float' and 'str'",
  "steps_to_reproduce": "Invoke execute_sdk_code with a valid Python script.",
  "expected_behavior": "Python script executes normally.",
  "actual_behavior": "Tool returns unsupported operand type(s) for +: 'float' and 'str' before user code runs.",
  "workaround": "None observed."
}
```

**Result:**

✅ Filed [#35](https://github.com/soumyapatro/foundry-bugbash-findings/issues/35) — [Known Limitation] SDK execute_sdk_code fails with unsupported operand type(s) for +: 'float' and 'str'

---

## Agent Summary

## Bug Bash Report: Create agent run and cancel it immediately

**Interface Tested:** SDK  
**Outcome:** ❌ Bug Found

### What Was Tested
I attempted to create an agent run and cancel it immediately using the Azure AI Foundry SDK, then poll the run until its status became `cancelled`.

### Result
The SDK execution did not reach the user script. The test runner failed immediately with:

`unsupported operand type(s) for +: 'float' and 'str'`

Because the script never executed, I could not verify the run status transition to `cancelled`.

### Issues Filed
- [#35](https://github.com/soumyapatro/foundry-bugbash-findings/issues/35) — `[Known Limitation] SDK execute_sdk_code fails with unsupported operand type(s) for +: 'float' and 'str'` (P3)

### Notes / Next Steps
- This blocked validation of the agent-run cancellation scenario.
- The issue is with the SDK execution environment/tooling, not the Foundry agent APIs themselves.
- If you want, I can retry the scenario via the REST API or CLI instead of the SDK.

---

## Issues Filed

- ⚠️ [#35](https://github.com/soumyapatro/foundry-bugbash-findings/issues/35) — [Known Limitation] SDK execute_sdk_code fails with unsupported operand type(s) for +: 'float' and 'str'

---

*🤖 Auto-generated by Bug Bash Agent · 2026-07-21 · [Foundry Project](https://ai.azure.com/nextgen/r/IUzq_Xd8QCibgX4i9dVEXQ,rg-foundry-bugbash,,bugbashstablemdqvfijmgy37s,proj-lifecycle/build/agents?tid=72f988bf-86f1-41af-91ab-2d7cd011db47)*