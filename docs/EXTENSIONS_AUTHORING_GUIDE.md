# Extensions Authoring Guide

This guide explains how to create LlamaGate Extensions from scratch. Extensions are YAML files that define structured AI tasks with inputs, outputs, prompts, rules, and guardrails.

## Extension Anatomy

A LlamaGate Extension consists of several key sections:

### 1. Metadata

Basic information about the extension:

```yaml
name: my_extension
version: 1.0.0
description: Brief description of what this extension does
author: Your Name
```

### 2. Inputs/Outputs

Define the expected input and output schemas using JSON Schema:

```yaml
inputs:
  type: object
  properties:
    text:
      type: string
      description: Input text to process
  required:
    - text

outputs:
  type: object
  properties:
    summary:
      type: string
      description: Generated summary
    confidence:
      type: number
      minimum: 0
      maximum: 1
  required:
    - summary
```

### 3. Task Type

Extensions can be either:
- **Model Task**: Uses an LLM to process inputs via prompts
- **Rules Task**: Uses rule-based logic (no LLM required)

```yaml
task:
  type: model  # or "rules"
```

### 4. Model Configuration (Model Tasks Only)

For model tasks, specify the LLM configuration:

```yaml
model:
  provider: ollama  # or your LLM provider
  model_name: llama3.2
  temperature: 0.7
  max_tokens: 1000
```

### 5. Prompt (Model Tasks)

Define the prompt template that guides the LLM:

```yaml
prompt: |
  You are a helpful assistant. Summarize the following text:
  
  {{inputs.text}}
  
  Provide a concise summary in 2-3 sentences.
```

### 6. Rules (Rules Tasks)

For rule-based tasks, define conditional logic:

```yaml
rules:
  - condition: "{{inputs.urgency}} == 'high'"
    action: route_to_urgent_queue
  - condition: "{{inputs.urgency}} == 'low'"
    action: route_to_normal_queue
```

### 7. Guardrails

Safety constraints to prevent unwanted behavior:

```yaml
guardrails:
  - type: no_network_access
    enabled: true
  - type: no_filesystem_access
    enabled: true
  - type: max_output_length
    value: 5000
```

### 8. Observability

Enable logging and metrics:

```yaml
observability:
  logging:
    enabled: true
    level: info
  metrics:
    enabled: true
    track_latency: true
```

## Copy/Paste Best Practices

### 1. Start from a Template

Use the provided templates as starting points:
- [`extension_minimal.yaml`](../templates/extension_minimal.yaml) - Bare minimum
- [`extension_model_task.yaml`](../templates/extension_model_task.yaml) - Model-based
- [`extension_rules_task.yaml`](../templates/extension_rules_task.yaml) - Rule-based

### 2. Validate Early and Often

- Check YAML syntax before testing
- Validate JSON Schema for inputs/outputs
- Test with sample inputs before production use

### 3. Use Safe Defaults

Always include guardrails:
- Disable network/filesystem access unless needed
- Set reasonable output length limits
- Enable logging for debugging

### 4. Document Your Extension

Include a README with:
- Purpose and use case
- Input/output examples
- Configuration requirements
- Known limitations

### 5. Test Incrementally

- Start with minimal inputs
- Verify outputs match schema
- Check logs for warnings/errors
- Gradually increase complexity

## Extension Checklist

Before deploying an extension, verify:

- [ ] YAML syntax is valid (no indentation errors)
- [ ] All required metadata fields are present
- [ ] Input/output schemas are valid JSON Schema
- [ ] Prompt template uses correct variable syntax (`{{inputs.field}}`)
- [ ] Model configuration matches available models
- [ ] Guardrails are enabled and appropriate
- [ ] Observability is configured for debugging
- [ ] README documents usage and examples
- [ ] Test inputs produce expected outputs
- [ ] No hardcoded secrets or sensitive data

## Validation Workflow

### Step 1: Syntax Validation

Use a YAML validator or your editor to check syntax:
- Proper indentation (spaces, not tabs)
- Correct key-value pairs
- Valid list/object structures

### Step 2: Schema Validation

Verify that your input/output schemas:
- Use valid JSON Schema syntax
- Match your test data
- Include required fields

### Step 3: Dry Run (if available)

If LlamaGate supports dry-run mode:
- Test without executing the model
- Validate prompt rendering
- Check rule conditions

### Step 4: Run with Test Inputs

Execute with known inputs:
- Use provided `test_input.json` files
- Compare outputs to `expected_output.json`
- Verify guardrails are respected

### Step 5: Log Review

Check logs for:
- Successful execution
- Any guardrail warnings
- Performance metrics
- Error messages

## Common Patterns

### Pattern 1: Text Summarization

```yaml
task:
  type: model
prompt: |
  Summarize the following text in 3 sentences:
  {{inputs.text}}
```

### Pattern 2: Structured Extraction

```yaml
task:
  type: model
prompt: |
  Extract the following information from the text:
  - Name
  - Email
  - Phone
  
  Text: {{inputs.text}}
  
  Return as JSON.
```

### Pattern 3: Conditional Routing

```yaml
task:
  type: rules
rules:
  - condition: "{{inputs.priority}} > 7"
    action: escalate
  - condition: "{{inputs.category}} == 'support'"
    action: route_to_support
```

## Next Steps

- Explore the [example extensions](../examples/extensions/) for real-world patterns
- Review the [AgenticModules Overview](AGENTICMODULES_OVERVIEW.md) to combine extensions
- Check [Troubleshooting](TROUBLESHOOTING.md) for common issues
