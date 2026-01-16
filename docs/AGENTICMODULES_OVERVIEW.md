# AgenticModules Overview

AgenticModules are orchestrated collections of LlamaGate Extensions that work together to accomplish complex workflows. This guide explains their structure, lifecycle, and execution.

## What is an AgenticModule?

An AgenticModule consists of:
1. **Manifest** (`agenticmodule.yaml`) - Defines the module's metadata, inputs/outputs, and workflow steps
2. **Extensions Folder** - Contains the YAML files for all extensions used in the module
3. **Tests Folder** (optional) - Sample inputs and expected outputs for validation

## Module Structure

### Recommended Layout

```
my_agenticmodule/
├── agenticmodule.yaml      # Module manifest
├── README.md               # Module documentation
├── extensions/             # Extension definitions
│   ├── step1_extension.yaml
│   ├── step2_extension.yaml
│   └── step3_extension.yaml
└── tests/                  # Test cases
    ├── sample_input_1.json
    ├── expected_1.json
    └── ...
```

### Manifest Structure

The `agenticmodule.yaml` file defines:

```yaml
name: my_module
version: 1.0.0
description: What this module does

inputs:
  type: object
  properties:
    # Module-level inputs
  required: [...]

outputs:
  type: object
  properties:
    # Module-level outputs
  required: [...]

workflow:
  steps:
    - name: step1
      extension: step1_extension.yaml
      inputs:
        # Map module inputs to extension inputs
    - name: step2
      extension: step2_extension.yaml
      inputs:
        # Can reference outputs from previous steps
        field: "{{steps.step1.outputs.result}}"
```

## Lifecycle States

AgenticModules progress through these states:

### 1. Proposed
- Initial design phase
- Workflow steps are conceptual
- Not yet implemented or tested

### 2. Designed
- Manifest and extensions are written
- Structure is defined
- Not yet validated

### 3. Validated
- Tested with sample inputs
- Outputs match expectations
- Guardrails verified
- Ready for limited use

### 4. Stable
- Production-ready
- Well-documented
- Tested across multiple scenarios
- Performance verified

### 5. Deprecated
- No longer recommended for new use
- May be replaced by newer modules
- Still functional but not maintained

## Module Execution

AgenticModules can be executed in two ways:

### Method 1: agenticmodule_runner

If your LlamaGate installation includes `agenticmodule_runner`:

```bash
agenticmodule_runner --module path/to/module --input input.json
```

The runner:
- Loads the manifest
- Resolves extension dependencies
- Executes steps in order
- Passes outputs between steps
- Returns final module output

### Method 2: Orchestrator Extension

If no dedicated runner exists, use an orchestrator extension that:
- Reads the `agenticmodule.yaml` manifest
- Loads and executes each extension in sequence
- Manages data flow between steps
- Handles errors and rollback if needed

## Workflow Step Execution

Steps execute sequentially by default:

1. **Step 1** receives module inputs (or mapped inputs)
2. **Step 1** executes its extension
3. **Step 1** produces outputs
4. **Step 2** receives Step 1 outputs (via `{{steps.step1.outputs.field}}`)
5. **Step 2** executes its extension
6. Process continues until all steps complete

### Input Mapping

Module inputs can be:
- Passed directly to a step: `field: "{{inputs.module_field}}"`
- Transformed: `field: "{{inputs.module_field | uppercase}}"`
- Combined: `field: "{{inputs.field1}} {{inputs.field2}}"`

### Output Referencing

Step outputs are referenced as:
- `{{steps.step_name.outputs.field_name}}`
- Only previous steps' outputs are available
- Module outputs are typically the final step's outputs

## Example: Intake and Routing

See the [`intake_and_routing`](../examples/agenticmodules/intake_and_routing/) example for a complete pattern:

1. **Step 1**: `intake_structured_summary` - Processes raw input into structured data
2. **Step 2**: `urgency_router` - Routes based on urgency from Step 1

The module manifest orchestrates these two extensions, passing the structured summary from Step 1 to Step 2's routing logic.

## Best Practices

### 1. Keep Steps Focused
Each extension should have a single, clear purpose. Complex logic should be split across multiple steps.

### 2. Document Data Flow
Clearly document how data flows between steps in your README:
- What each step expects
- What each step produces
- How outputs map to next step inputs

### 3. Handle Errors
Consider error handling:
- What happens if a step fails?
- Should the module continue or abort?
- How are errors surfaced?

### 4. Test Each Step
Test extensions individually before testing the full module:
- Ensures each step works in isolation
- Makes debugging easier
- Validates step contracts

### 5. Version Your Modules
Use semantic versioning:
- `1.0.0` - Initial stable release
- `1.1.0` - New features, backward compatible
- `2.0.0` - Breaking changes

## Module vs. Extension

| Aspect | Extension | AgenticModule |
|--------|-----------|---------------|
| Scope | Single task | Multiple orchestrated tasks |
| File | Single YAML | Manifest + extension folder |
| Execution | Direct | Via runner or orchestrator |
| Use Case | Simple, isolated task | Complex workflow |
| Complexity | Low | Medium to High |

## Next Steps

- Explore the [intake_and_routing example](../examples/agenticmodules/intake_and_routing/)
- Review the [minimal module template](../templates/agenticmodule_minimal.yaml)
- Read the [Extensions Authoring Guide](EXTENSIONS_AUTHORING_GUIDE.md) to build module components
