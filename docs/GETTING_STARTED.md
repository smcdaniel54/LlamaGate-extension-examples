# Getting Started

This guide will help you understand how to use the examples in this repository to build your own LlamaGate Extensions and AgenticModules.

## Prerequisites

Before using these examples, ensure you have:

1. **LlamaGate installed and running** - The main LlamaGate application should be installed and accessible
2. **Access to a local LLM** - LlamaGate requires a configured local LLM (e.g., via Ollama, LM Studio, or similar)
3. **YAML editor** - Any text editor that supports YAML syntax highlighting
4. **Basic understanding of YAML** - Familiarity with YAML structure and syntax

## How to Choose an Example

### Start with Extensions if you:
- Are new to LlamaGate
- Want to understand basic extension structure
- Need a single-purpose task (e.g., text summarization, data extraction)

**Recommended first example:** [`intake_structured_summary`](../examples/extensions/intake_structured_summary/)

### Move to AgenticModules if you:
- Understand extension basics
- Need to orchestrate multiple steps
- Want to chain extensions together

**Recommended first module:** [`intake_and_routing`](../examples/agenticmodules/intake_and_routing/)

## How to Validate and Run Examples

### 1. Validate the YAML Structure

Before running, validate that your YAML is syntactically correct:
- Check for proper indentation (YAML is whitespace-sensitive)
- Ensure all required fields are present (see [Extensions Authoring Guide](EXTENSIONS_AUTHORING_GUIDE.md))
- Verify that input/output schemas are valid JSON Schema

### 2. Review the Example README

Each example includes a `README.md` that describes:
- What the extension/module does
- Required inputs and expected outputs
- How to configure it for your environment
- What "good output" looks like

### 3. Prepare Test Inputs

Most examples include `test_input.json` files. Review these to understand the expected input format, then:
- Modify inputs to match your use case
- Ensure inputs match the schema defined in the extension YAML

### 4. Run the Extension/Module

The exact CLI commands depend on your LlamaGate installation. Generally:

**For Extensions:**
- Extensions are typically discovered automatically when placed in the configured extensions directory
- You may need to trigger them via API, CLI, or through the LlamaGate interface
- Check LlamaGate documentation for the current execution method

**For AgenticModules:**
- Modules may be executed via `agenticmodule_runner` (if available in your LlamaGate version)
- Or via an orchestrator extension that processes the module manifest
- Refer to [AgenticModules Overview](AGENTICMODULES_OVERVIEW.md) for execution details

### 5. Review Outputs

Compare your outputs against the `expected_output.json` files included with each example:
- Check that required fields are present
- Verify data types match the schema
- Ensure guardrails were respected (no violations in logs)

## Where to See Logs and Outputs

### Logs Location

LlamaGate logs are typically found in:
- Application logs directory (varies by installation)
- Console output (if running in terminal)
- Log files may be named `llamagate.log` or similar

### Output Location

Extension/module outputs may be:
- Returned via API response
- Written to configured output directories
- Displayed in the LlamaGate interface
- Logged to console or log files

### What to Look For

When reviewing logs:
- ✅ Successful execution messages
- ⚠️ Guardrail warnings (may be expected)
- ❌ Errors (model not found, schema validation failures, etc.)
- 📊 Observability metrics (if enabled in the extension)

## Next Steps

- Read the [Extensions Authoring Guide](EXTENSIONS_AUTHORING_GUIDE.md) to build your own extensions
- Explore the [AgenticModules Overview](AGENTICMODULES_OVERVIEW.md) for orchestration patterns
- Check [Troubleshooting](TROUBLESHOOTING.md) if you encounter issues
