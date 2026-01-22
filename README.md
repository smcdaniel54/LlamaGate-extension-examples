# LlamaGate Extension Examples

A companion repository providing simple, high-value, copy/paste-ready examples of LlamaGate Extensions (YAML) and AgenticModules (extension bundles with orchestration). These examples demonstrate real-world patterns for building local LLM workflows, MCP automation, and structured AI task processing.

**Main Repository:** [LlamaGate](https://github.com/smcdaniel54/LlamaGate)

## Start Here

### Extensions (Beginner-Friendly)
Extensions are single-purpose YAML configurations that define inputs, outputs, prompts, rules, and guardrails for LLM tasks. Start with the [minimal extension template](templates/extension_minimal.yaml) or explore the [intake_structured_summary](examples/extensions/intake_structured_summary/) example.

### AgenticModules (Bundled Capability)
AgenticModules combine multiple extensions into orchestrated workflows. Each module includes a manifest (`agenticmodule.yaml`) and a folder of related extensions. See the [intake_and_routing](examples/agenticmodules/intake_and_routing/) example for a complete pattern.

## Examples

| Name | Type | What It Does | Path |
|------|------|--------------|------|
| Structured Intake Summary | Extension | Converts raw request text into structured JSON (summary, intent, urgency, action items) | `examples/extensions/intake_structured_summary/` |
| ROI Assessment | Extension | Calculates ROI from costs, savings, and risk factors; returns decision recommendation | `examples/extensions/roi_assessment_minimal/` |
| Intake and Routing | AgenticModule | Orchestrates intake processing and urgency-based routing using two extensions | `examples/agenticmodules/intake_and_routing/` |
| Text Analyzer | AgenticModule | Extracts key information and analyzes sentiment from text using two extensions | `examples/agenticmodules/text_analyzer/` |

## Quick Links

- 📖 [Getting Started Guide](docs/GETTING_STARTED.md) - How to use these examples
- ✍️ [Extensions Authoring Guide](docs/EXTENSIONS_AUTHORING_GUIDE.md) - Build your own extensions
- 🔄 [AgenticModules Overview](docs/AGENTICMODULES_OVERVIEW.md) - Understand module structure and lifecycle
- 🔧 [Troubleshooting](docs/TROUBLESHOOTING.md) - Common issues and solutions

## Installing AgenticModules

### Using agentic-module-tool (Recommended)

The [agentic-module-tool](https://github.com/smcdaniel54/agentic-module-tool) provides automated installation and testing for AgenticModules:

```bash
# Validate module
agentic-module-tool validate text_analyzer \
  --modules-dir examples/agenticmodules

# Install to LlamaGate
agentic-module-tool install text_analyzer \
  --modules-dir examples/agenticmodules \
  --llamagate-ext-dir ../LlamaGate/extensions

# Test module
agentic-module-tool test text_analyzer \
  --modules-dir examples/agenticmodules \
  --llamagate-url http://localhost:11435
```

### Manual Installation

Copy the module directory to your LlamaGate extensions directory and restart LlamaGate. See [Getting Started Guide](docs/GETTING_STARTED.md) for detailed instructions.

## Templates

Ready-to-use templates for quick starts:

- [`extension_minimal.yaml`](templates/extension_minimal.yaml) - Bare minimum extension structure
- [`extension_model_task.yaml`](templates/extension_model_task.yaml) - Model-based task with prompt
- [`extension_rules_task.yaml`](templates/extension_rules_task.yaml) - Rule-based routing structure
- [`agenticmodule_minimal.yaml`](templates/agenticmodule_minimal.yaml) - Minimal AgenticModule manifest

## Contributing

Found a bug or have an example to share? Contributions welcome! Please ensure all examples include:
- Clear README with inputs/outputs
- Test input/output files
- Safe defaults (no network/filesystem access unless documented)

---

*Note: AgenticModules were formerly referred to as agentic workflows in early drafts.*
