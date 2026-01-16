# Intake and Routing AgenticModule

This AgenticModule orchestrates a two-step workflow: first processing raw request text into structured data, then routing based on urgency and intent.

## What It Does

The module combines two extensions to create an automated intake and routing system:

1. **Intake Processing** - Converts unstructured request text into structured JSON with summary, intent, urgency, and action items
2. **Urgency Routing** - Uses rule-based logic to route requests to appropriate queues based on urgency level and intent

## Workflow Steps

### Step 1: Intake Processing
- **Extension:** `intake_structured_summary.yaml`
- **Input:** Raw request text from module input
- **Output:** Structured summary with fields: summary, intent, urgency, action_items

### Step 2: Urgency Routing
- **Extension:** `urgency_router.yaml`
- **Input:** Urgency, intent, and summary from Step 1
- **Output:** Routing decision with route, priority, and reasoning

## Inputs

- **request_text** (string, required): Raw text of the incoming request or ticket

## Outputs

- **structured_summary** (object): Complete structured summary from intake step
  - summary (string)
  - intent (string)
  - urgency (string)
  - action_items (array of strings)
- **routing_decision** (object): Routing decision from router step
  - route (string): Target queue or team
  - priority (string): Priority level
  - reasoning (string): Explanation of routing decision

## How to Run

1. Ensure LlamaGate is running with access to a local LLM
2. Place this module directory in your LlamaGate modules directory (or configured location)
3. Ensure both extension files are in the `extensions/` subdirectory
4. Provide input in the format shown in `tests/sample_input_1.json`
5. Execute the module via `agenticmodule_runner` (if available) or orchestrator extension
6. Review the combined output with both structured summary and routing decision

## Data Flow

```
Module Input (request_text)
    ↓
Step 1: Intake Processing
    ↓
Structured Summary (urgency, intent, summary)
    ↓
Step 2: Urgency Routing
    ↓
Module Output (structured_summary + routing_decision)
```

## What "Good Output" Looks Like

A successful output should:
- ✅ Contain both `structured_summary` and `routing_decision` objects
- ✅ Have a valid urgency value from Step 1 ("low", "medium", "high", "critical")
- ✅ Include a routing decision with route, priority, and reasoning
- ✅ Show routing logic that matches the urgency level
- ✅ Be valid JSON that can be parsed

### Example Good Output

```json
{
  "structured_summary": {
    "summary": "User reports critical login issue on mobile devices blocking urgent meeting access.",
    "intent": "bug_report",
    "urgency": "high",
    "action_items": [
      "Investigate mobile browser compatibility",
      "Check server logs for mobile errors",
      "Test on multiple mobile devices"
    ]
  },
  "routing_decision": {
    "route": "urgent_engineering_queue",
    "priority": "high",
    "reasoning": "High urgency bug report requires immediate engineering attention"
  }
}
```

## Routing Logic

The urgency router uses the following rules:
- **Critical urgency** → `critical_engineering_queue` (priority: critical)
- **High urgency** → `urgent_engineering_queue` (priority: high)
- **Medium urgency** → `normal_support_queue` (priority: medium)
- **Low urgency** → `standard_queue` (priority: low)
- **Intent-based overrides:** Feature requests route to `product_queue` regardless of urgency

## Test Files

- `tests/sample_input_1.json`: High urgency bug report
- `tests/sample_input_2.json`: Low urgency feature request
- `tests/expected_1.json`: Expected output for sample 1
- `tests/expected_2.json`: Expected output for sample 2

## Customization

### Adding More Routing Rules

Edit `extensions/urgency_router.yaml` to add:
- Additional urgency levels
- Intent-based routing rules
- Priority escalation logic

### Modifying Intake Processing

Edit `extensions/intake_structured_summary.yaml` to:
- Change extraction criteria
- Add new fields to structured output
- Adjust urgency assessment logic

### Adding Steps

To add more steps to the workflow:
1. Create a new extension in `extensions/`
2. Add a step to `agenticmodule.yaml` workflow
3. Map inputs from previous steps or module inputs

## Limitations

- Routing is rule-based and may not handle edge cases
- Urgency assessment depends on LLM interpretation
- No feedback loop for routing accuracy
- Sequential execution only (no parallel steps)

## Next Steps

- Review individual extensions to understand their behavior
- Test with your own request samples
- Customize routing rules for your organization's needs
- Consider adding additional steps (e.g., notification, escalation)
