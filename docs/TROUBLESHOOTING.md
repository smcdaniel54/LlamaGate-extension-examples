# Troubleshooting

Common issues when working with LlamaGate Extensions and AgenticModules, and how to resolve them.

## Common YAML Errors

### Indentation Issues

**Symptom:** YAML parser errors, "mapping values are not allowed here"

**Solution:**
- YAML requires consistent indentation (spaces, not tabs)
- Use 2 spaces per level (recommended)
- Ensure all keys at the same level have the same indentation

**Example:**
```yaml
# ❌ Wrong (mixed indentation)
inputs:
  type: object
    properties:  # Too much indentation
      field: string

# ✅ Correct
inputs:
  type: object
  properties:
    field: string
```

### Missing Required Fields

**Symptom:** Extension not discovered or validation errors

**Solution:**
- Ensure all required metadata fields are present: `name`, `version`, `description`
- Verify `inputs` and `outputs` sections exist
- Check that `task.type` is specified

### Invalid JSON Schema

**Symptom:** Schema validation errors

**Solution:**
- Validate your JSON Schema syntax (use an online validator)
- Ensure `type` is specified for all properties
- Check that `required` arrays only contain property names that exist

## Model Not Found

### Symptom
Error messages like "Model 'llama3.2' not found" or "Provider 'ollama' not available"

### Solutions

1. **Verify Model Name**
   - Check that the model name in your extension matches exactly (case-sensitive)
   - Ensure the model is installed in your LLM provider

2. **Check Provider Configuration**
   - Verify your LLM provider (Ollama, LM Studio, etc.) is running
   - Confirm the provider name in your extension matches your LlamaGate configuration

3. **Test Model Availability**
   - Try listing available models via your provider's CLI/API
   - Verify LlamaGate can access the provider

4. **Update Extension**
   - Change `model.model_name` to a model you know is available
   - Or update `model.provider` to match your setup

## Extension Not Discovered

### Symptom
Extension doesn't appear in LlamaGate or isn't executed

### Solutions

1. **Check File Location**
   - Ensure the extension is in the configured extensions directory
   - Verify the file has a `.yaml` or `.yml` extension

2. **Validate YAML Syntax**
   - Run a YAML validator on your file
   - Check for syntax errors that prevent parsing

3. **Verify Metadata**
   - Ensure `name` field is present and unique
   - Check that `version` is specified

4. **Check LlamaGate Configuration**
   - Verify extensions directory path in LlamaGate config
   - Ensure file permissions allow reading

5. **Restart LlamaGate**
   - Extensions are typically loaded at startup
   - Restart to pick up new or modified extensions

## Guardrail Violations

### Symptom
Warnings or errors about guardrail violations in logs

### Common Violations

1. **Network Access Attempted**
   - **Cause:** Extension tries to make HTTP requests
   - **Solution:** Ensure `guardrails.no_network_access.enabled: true` if network isn't needed, or remove the guardrail if network access is required

2. **Filesystem Access Attempted**
   - **Cause:** Extension tries to read/write files
   - **Solution:** Similar to network access - enable guardrail if not needed, or remove if required

3. **Output Length Exceeded**
   - **Cause:** LLM output exceeds `max_output_length` limit
   - **Solution:** Increase the limit or adjust the prompt to request shorter outputs

### Handling Violations

- **Expected Violations:** Some guardrails may warn but allow execution (check guardrail configuration)
- **Blocking Violations:** Others may stop execution - review logs to identify the cause
- **Adjust Guardrails:** Modify guardrail settings in your extension if needed

## Schema Validation Failures

### Symptom
"Input validation failed" or "Output does not match schema"

### Solutions

1. **Check Input Data**
   - Verify your input JSON matches the `inputs` schema exactly
   - Ensure required fields are present
   - Check data types (string vs number, etc.)

2. **Check Output Data**
   - Review actual outputs vs. expected schema
   - Ensure LLM is producing the correct format
   - Consider adding output parsing/transformation

3. **Validate Schemas**
   - Use a JSON Schema validator to test your schemas
   - Ensure schemas are valid JSON Schema (not just JSON)

## Module Execution Issues

### Symptom
AgenticModule fails to execute or steps don't run

### Solutions

1. **Check Manifest Syntax**
   - Validate `agenticmodule.yaml` syntax
   - Ensure workflow steps are properly defined

2. **Verify Extension References**
   - Check that extension files referenced in steps exist
   - Ensure paths are correct (relative to manifest)

3. **Check Step Input Mapping**
   - Verify input mappings use correct syntax: `{{inputs.field}}` or `{{steps.step_name.outputs.field}}`
   - Ensure referenced step outputs exist

4. **Review Step Execution Order**
   - Steps execute sequentially
   - Ensure steps that produce outputs are executed before steps that consume them

## Where Logs Live

### Log Locations

Logs are typically found in:

1. **Application Log Directory**
   - Varies by installation (check LlamaGate documentation)
   - Common locations: `~/.llamagate/logs/`, `./logs/`, or system log directory

2. **Console Output**
   - If running LlamaGate in terminal, logs appear in stdout/stderr
   - Look for execution messages, errors, and warnings

3. **Log Files**
   - May be named `llamagate.log`, `extensions.log`, or similar
   - Check LlamaGate configuration for exact paths

### Log Levels

- **DEBUG:** Detailed execution information
- **INFO:** General execution messages
- **WARN:** Guardrail violations, non-fatal issues
- **ERROR:** Failures that prevent execution

### What to Look For

When troubleshooting, check logs for:
- Extension/module loading messages
- Execution start/end timestamps
- Guardrail warnings
- Schema validation errors
- Model provider errors
- Step execution status (for modules)

## Performance Issues

### Symptom
Extensions/modules run slowly or time out

### Solutions

1. **Check Model Configuration**
   - Reduce `max_tokens` if outputs are too long
   - Adjust `temperature` (lower = faster, but less creative)
   - Consider using a smaller/faster model

2. **Review Prompt Length**
   - Shorter prompts process faster
   - Remove unnecessary context if possible

3. **Enable Observability**
   - Check latency metrics to identify bottlenecks
   - Review which steps take longest (for modules)

4. **Optimize Rules Tasks**
   - Rules tasks are faster than model tasks
   - Use rules where possible instead of LLM calls

## Getting Help

If issues persist:

1. **Check Documentation**
   - Review [Getting Started](GETTING_STARTED.md)
   - Consult [Extensions Authoring Guide](EXTENSIONS_AUTHORING_GUIDE.md)

2. **Review Examples**
   - Compare your extension/module to working examples
   - Ensure you're following the same patterns

3. **Validate Against Templates**
   - Use provided templates as reference
   - Ensure your structure matches

4. **Check LlamaGate Version**
   - Some features may require specific versions
   - Verify compatibility with your installation
