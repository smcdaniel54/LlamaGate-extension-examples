# Webhook Receiver Extension: Examples Repo Implementation Plan

## Overview

This plan outlines the process for adding the `github-webhook-receiver` extension to the `LlamaGate-extension-examples` repository **after** dynamic endpoints are implemented in LlamaGate.

## Prerequisites

### Required Before Implementation

1. **✅ Dynamic Endpoints Feature Implemented in LlamaGate**
   - LlamaGate must support `endpoints:` field in extension manifests
   - Route registration must work
   - Extension endpoints must be accessible
   - See: `../LlamaGate/docs/DYNAMIC_ENDPOINTS_IMPLEMENTATION_PLAN.md`

2. **✅ Extension Tested and Working**
   - Extension must work with dynamic endpoints
   - Webhook receiving must function
   - LLM processing must work
   - Event storage must work

3. **✅ Documentation Updated**
   - README must reflect working state
   - Remove "not yet implemented" warnings
   - Add working examples

## Target Repository

**Repository**: `LlamaGate-extension-examples`  
**URL**: `https://github.com/smcdaniel54/LlamaGate-extension-examples`  
**Local Path**: `../LlamaGate-extension-examples/`

## Target Location

```
LlamaGate-extension-examples/
└── examples/
    └── extensions/
        └── github-webhook-receiver/
            ├── extension.yaml
            ├── README.md
            ├── test_input.json
            ├── expected_output.json
            └── test_webhook.sh
```

## Source Location

**Current Location**: `OrchestratorPlus/extensions/github-webhook-receiver/`

**Files Available**:
- `extension.yaml` (or `manifest.yaml` in source) - Extension definition with dynamic endpoint
- `README.md` - Complete documentation

## Implementation Phases

### Phase 1: Prepare Extension (Current Status)

**Status**: ✅ Complete

- [x] Extension manifest created
- [x] Dynamic endpoint defined
- [x] Workflow steps implemented
- [x] README documentation written
- [x] Requirements documented

**Files in OrchestratorPlus**:
- `extensions/github-webhook-receiver/manifest.yaml` (will be renamed to `extension.yaml` for examples repo)
- `extensions/github-webhook-receiver/README.md`

### Phase 2: Wait for LlamaGate Implementation

**Status**: ⏳ Waiting

**Blockers**:
- [ ] Dynamic endpoints feature implemented in LlamaGate
- [ ] Route registration working
- [ ] Extension endpoints accessible

**What to Monitor**:
- LlamaGate repository for dynamic endpoints PR/commit
- LlamaGate release notes for feature announcement
- Test dynamic endpoints with simple extension first

### Phase 3: Test Extension Locally

**Status**: ⏳ Pending

**Steps**:

1. **Verify Dynamic Endpoints Work**
   ```bash
   # Test with simple extension first
   # Then test webhook receiver
   ```

2. **Test Webhook Receiver**
   ```bash
   # Install extension to LlamaGate
   cp -r extensions/github-webhook-receiver /path/to/LlamaGate/extensions/
   
   # Restart LlamaGate
   # Verify endpoint is accessible
   curl -X POST http://localhost:11435/v1/extensions/github-webhook-receiver/webhooks/github \
     -H "Content-Type: application/json" \
     -d @test_input.json
   ```

3. **Validate Workflow**
   - Verify LLM analysis works
   - Verify event storage works
   - Verify response generation works

### Phase 4: Adapt for Examples Repo

**Status**: ⏳ Pending

**Changes Needed**:

1. **Update README**
   - Remove "not yet implemented" warnings
   - Add working setup instructions
   - Update examples with real URLs
   - Add troubleshooting section
   - Simplify for copy/paste use

2. **Add Test Files**
   - `test_input.json` - Sample GitHub webhook payload
   - `expected_output.json` - Expected response
   - `test_webhook.sh` - Test script for manual testing

3. **Simplify for Examples**
   - Ensure it's copy/paste ready
   - Rename `manifest.yaml` to `extension.yaml` (examples repo convention)
   - Add clear comments in extension.yaml
   - Include minimal config
   - Remove OrchestratorPlus-specific references
   - Verify uses LlamaGate standard model (`mistral`) for testing
   - Follow existing README structure pattern (What It Does, Inputs, Outputs, How to Run, Configuration, What "Good Output" Looks Like)

### Phase 5: Copy to Examples Repo

**Status**: ⏳ Pending

**Target Location**:
```
LlamaGate-extension-examples/
└── examples/
    └── extensions/
        └── github-webhook-receiver/
            ├── extension.yaml
            ├── README.md
            ├── test_input.json
            ├── expected_output.json
            └── test_webhook.sh
```

**Files to Copy**:
- `extension.yaml` (renamed from `manifest.yaml`, may need minor adjustments)
- `README.md` (updated for examples repo)
- Test files (new)

**Copy Command**:
```bash
# From OrchestratorPlus directory
cp -r extensions/github-webhook-receiver \
  ../LlamaGate-extension-examples/examples/extensions/
```

### Phase 6: Update Examples Repo Documentation

**Status**: ⏳ Pending

**Files to Update**:

1. **`README.md`** (Main)
   - Add to examples table:
     ```markdown
     | GitHub Webhook Receiver | Extension | Receives GitHub webhooks and processes with LLM | `examples/extensions/github-webhook-receiver/` |
     ```

2. **`docs/GETTING_STARTED.md`**
   - Add section on dynamic endpoints
   - Show webhook receiver as example
   - Include setup instructions
   - Show how to configure GitHub webhook

3. **`docs/EXTENSIONS_AUTHORING_GUIDE.md`** (if exists)
   - Add section on dynamic endpoints
   - Show webhook receiver pattern
   - Document endpoint definition
   - Show examples

### Phase 7: Create PR

**Status**: ⏳ Pending

**PR Contents**:
- New extension directory
- Updated documentation
- Test files
- Example usage

**PR Description Template**:
```markdown
## Add GitHub Webhook Receiver Extension Example

### New Extension: github-webhook-receiver

Demonstrates dynamic endpoints feature with a high-value, real-world use case.

**Features:**
- Receives GitHub webhooks via custom endpoint (`POST /v1/extensions/github-webhook-receiver/webhooks/github`)
- LLM-powered event analysis using mistral (LlamaGate standard model for testing)
- Event storage for audit trail
- Friendly response generation

**Demonstrates:**
- Dynamic endpoint definition (`endpoints:` field)
- Webhook integration pattern
- LLM processing workflow
- File storage pattern
- Real-world integration use case

### Documentation Updates

- Added to examples table in README
- Added dynamic endpoints section to GETTING_STARTED.md
- Complete setup and usage instructions
- GitHub webhook configuration guide

### Testing

Extension tested and verified working with dynamic endpoints feature.
Includes test files for manual testing.

### Requirements

- LlamaGate with dynamic endpoints feature (v0.9.2+)
- Ollama with mistral (LlamaGate standard model for testing)
- GitHub repository (for webhook configuration)
```

## File Structure in Examples Repo

```
LlamaGate-extension-examples/
├── examples/
│   └── extensions/
│       └── github-webhook-receiver/
│           ├── extension.yaml          # Extension definition (examples repo convention)
│           ├── README.md               # Documentation
│           ├── test_input.json         # Sample webhook payload
│           ├── expected_output.json    # Expected response
│           └── test_webhook.sh        # Test script (optional, for manual testing)
├── README.md                            # UPDATE: Add to table
└── docs/
    └── GETTING_STARTED.md               # UPDATE: Add dynamic endpoints section
```

## Key Differences: OrchestratorPlus vs Examples Repo

### OrchestratorPlus (Current)
- **Purpose**: Production/development extension
- **Documentation**: Full, includes "not yet implemented" warnings
- **Location**: `extensions/github-webhook-receiver/`
- **Status**: Awaiting feature implementation
- **Audience**: Developers working on OrchestratorPlus

### Examples Repo (Target)
- **Purpose**: Educational example
- **Documentation**: Simplified, working examples only
- **Location**: `examples/extensions/github-webhook-receiver/`
- **File Name**: `extension.yaml` (not `manifest.yaml` - follows examples repo convention)
- **Model**: Uses LlamaGate standard model (`mistral`) for testing
- **Status**: Ready to use (after feature implemented)
- **Additional**: Test files, simplified config
- **Audience**: Extension authors learning patterns
- **README Structure**: Follows examples repo pattern (What It Does, Inputs, Outputs, How to Run, Configuration, What "Good Output" Looks Like)

## Test Files to Create

### 1. `test_input.json`

Sample GitHub webhook payload:

```json
{
  "action": "opened",
  "repository": {
    "full_name": "owner/repo",
    "name": "repo",
    "description": "Test repository"
  },
  "sender": {
    "login": "testuser",
    "type": "User"
  },
  "pull_request": {
    "title": "Test Pull Request",
    "number": 123,
    "body": "This is a test PR"
  }
}
```

### 2. `expected_output.json`

Expected response structure:

```json
{
  "success": true,
  "data": {
    "processed_event": {
      "summary": "Pull request #123 opened by testuser in owner/repo",
      "action": "opened",
      "repository": "owner/repo"
    },
    "response_message": "Received and processed pull request event. Event stored and analyzed successfully.",
    "event_stored": true,
    "event_id": "owner-repo-2026-01-22T10-30-45-opened"
  }
}
```

### 3. `test_webhook.sh` (Optional)

Test script for manual testing (note: requires `jq` for JSON parsing):

```bash
#!/bin/bash

# Test GitHub Webhook Receiver Extension
# Requires: LlamaGate running with dynamic endpoints support

LLAMAGATE_URL="${LLAMAGATE_URL:-http://localhost:11435}"
ENDPOINT="${LLAMAGATE_URL}/v1/extensions/github-webhook-receiver/webhooks/github"

echo "Testing GitHub Webhook Receiver Extension"
echo "Endpoint: ${ENDPOINT}"
echo ""

# Send test webhook
response=$(curl -s -X POST "${ENDPOINT}" \
  -H "Content-Type: application/json" \
  -H "X-GitHub-Event: pull_request" \
  -d @test_input.json)

echo "Response:"
echo "${response}" | jq '.'

# Check if successful
if echo "${response}" | jq -e '.success == true' > /dev/null; then
    echo ""
    echo "✅ Test passed!"
    exit 0
else
    echo ""
    echo "❌ Test failed!"
    exit 1
fi
```

## Checklist

### Before Adding to Examples Repo

- [ ] Dynamic endpoints feature implemented in LlamaGate
- [ ] Extension tested and working
- [ ] README updated (remove warnings, follow examples repo structure)
- [ ] File renamed: `manifest.yaml` → `extension.yaml` (examples repo convention)
- [ ] Model updated to LlamaGate standard model (`mistral`) for testing
- [ ] Test files created
- [ ] Documentation simplified for examples
- [ ] Verified copy/paste ready
- [ ] Tested installation from examples repo
- [ ] Guardrails follow examples repo pattern (no network/filesystem access unless documented)

### When Adding to Examples Repo

- [ ] Copy files to `examples/extensions/github-webhook-receiver/`
- [ ] Update main `README.md` (add to table)
- [ ] Update `docs/GETTING_STARTED.md` (add section)
- [ ] Update `docs/EXTENSIONS_AUTHORING_GUIDE.md` (if exists)
- [ ] Test installation from examples repo
- [ ] Verify endpoint works
- [ ] Create PR with clear description

## Timeline

### Current (Now)
- ✅ Extension created in OrchestratorPlus
- ✅ Documentation written
- ⏳ Waiting for LlamaGate dynamic endpoints

### After LlamaGate Implementation
1. **Week 1**: Test extension locally
2. **Week 2**: Adapt for examples repo
3. **Week 3**: Copy and update documentation
4. **Week 4**: Create PR

## Source Repository

**Current Location**: `OrchestratorPlus/extensions/github-webhook-receiver/`

**Files to Copy**:
- `manifest.yaml` → rename to `extension.yaml` for examples repo
- `README.md` (with updates to follow examples repo structure)

**Files to Create**:
- `test_input.json`
- `expected_output.json`
- `test_webhook.sh`

## Documentation Updates Required

### Main README.md

Add to examples table:

```markdown
| GitHub Webhook Receiver | Extension | Receives GitHub webhooks and processes with LLM | `examples/extensions/github-webhook-receiver/` |
```

### docs/GETTING_STARTED.md

Add section:

```markdown
## Dynamic Endpoints

LlamaGate supports dynamic endpoints, allowing extensions to define custom HTTP endpoints.

### Example: GitHub Webhook Receiver

The `github-webhook-receiver` extension demonstrates dynamic endpoints:

1. **Install the extension**:
   ```bash
   cp -r examples/extensions/github-webhook-receiver /path/to/LlamaGate/extensions/
   ```

2. **Restart LlamaGate** to register the endpoint

3. **Configure GitHub webhook**:
   - Go to repository Settings → Webhooks
   - Add webhook: `https://your-server.com/v1/extensions/github-webhook-receiver/webhooks/github`
   - Select events to receive

4. **Test the webhook**:
   ```bash
   curl -X POST http://localhost:11435/v1/extensions/github-webhook-receiver/webhooks/github \
     -H "Content-Type: application/json" \
     -H "X-GitHub-Event: pull_request" \
     -d @test_input.json
   ```

**Note**: This extension uses the LlamaGate standard model `mistral` for testing. Update the model in `extension.yaml` if you prefer a different model.

See [github-webhook-receiver README](../examples/extensions/github-webhook-receiver/README.md) for details.
```

## Next Steps

1. **Monitor LlamaGate** for dynamic endpoints implementation
2. **Test extension** once feature is available
3. **Adapt documentation** for examples repo format
4. **Create test files** for examples repo
5. **Copy to examples repo** and update documentation
6. **Create PR** to examples repo

## Success Criteria

- [ ] Extension works with dynamic endpoints
- [ ] Test files included and working
- [ ] Documentation updated in examples repo
- [ ] Extension appears in examples table
- [ ] Getting started guide includes dynamic endpoints
- [ ] PR created and ready for review

---

**Status**: ⏳ Awaiting LlamaGate dynamic endpoints feature implementation

**Last Updated**: 2026-01-22

**Related Documents**:
- `../LlamaGate/docs/DYNAMIC_ENDPOINTS_IMPLEMENTATION_PLAN.md` - LlamaGate implementation plan
- `../../OrchestratorPlus/extensions/github-webhook-receiver/` - Source extension
