# Text Analyzer AgenticModule

This AgenticModule demonstrates a simple two-step workflow for analyzing text: extracting key information and determining sentiment. It's designed to be beginner-friendly and easy to understand.

## What It Does

The module combines two extensions to create a complete text analysis system:

1. **Information Extraction** - Extracts key points, topics, and entities from text
2. **Sentiment Analysis** - Determines the sentiment (positive, neutral, negative) with confidence score

## Workflow Steps

### Step 1: Information Extraction
- **Extension:** `text_extractor.yaml`
- **Input:** Raw text from module input
- **Output:** Structured information with key_points, topics, and entities

### Step 2: Sentiment Analysis
- **Extension:** `sentiment_analyzer.yaml`
- **Input:** Original text and key points from Step 1
- **Output:** Sentiment classification with confidence and reasoning

## Inputs

- **text** (string, required): Text to analyze

## Outputs

- **extracted_info** (object): Key information extracted from the text
  - key_points (array of strings): Main points or ideas
  - topics (array of strings): Topics or themes identified
  - entities (array of strings): Named entities found
- **sentiment_analysis** (object): Sentiment analysis results
  - sentiment (string): "positive", "neutral", or "negative"
  - confidence (number): Confidence score from 0.0 to 1.0
  - reasoning (string): Explanation of the sentiment assessment

## Installation

### Using agentic-module-tool (Recommended)

1. **Validate the module**
   ```bash
   agentic-module-tool validate text_analyzer \
     --modules-dir examples/agenticmodules
   ```

2. **Install to LlamaGate**
   ```bash
   agentic-module-tool install text_analyzer \
     --modules-dir examples/agenticmodules \
     --llamagate-ext-dir ../LlamaGate/extensions
   ```

3. **Test the module**
   ```bash
   agentic-module-tool test text_analyzer \
     --modules-dir examples/agenticmodules \
     --llamagate-url http://localhost:11435
   ```

### Manual Installation

1. Copy the `text_analyzer` directory to your LlamaGate extensions directory
2. Ensure both extension files are in the `extensions/` subdirectory
3. Restart LlamaGate to discover the new extensions

## How to Run

1. Ensure LlamaGate is running with access to a local LLM (e.g., Ollama with mistral)
2. Install the module using the steps above
3. Provide input in the format shown in `tests/sample_input_1.json`
4. Execute the module via `agenticmodule_runner` (if available) or orchestrator extension
5. Review the combined output with both extracted information and sentiment analysis

## Data Flow

```
Module Input (text)
    ↓
Step 1: Information Extraction
    ↓
Extracted Info (key_points, topics, entities)
    ↓
Step 2: Sentiment Analysis
    ↓
Module Output (extracted_info + sentiment_analysis)
```

## What "Good Output" Looks Like

A successful output should:
- ✅ Contain both `extracted_info` and `sentiment_analysis` objects
- ✅ Have valid key_points array (3-5 items)
- ✅ Have valid topics array (2-4 items)
- ✅ Include sentiment value ("positive", "neutral", or "negative")
- ✅ Include confidence score between 0.0 and 1.0
- ✅ Include reasoning explanation
- ✅ Be valid JSON that can be parsed

### Example Good Output

```json
{
  "extracted_info": {
    "key_points": [
      "New product launch is happening",
      "Features are impressive",
      "Team has done excellent work"
    ],
    "topics": [
      "product launch",
      "customer satisfaction"
    ],
    "entities": []
  },
  "sentiment_analysis": {
    "sentiment": "positive",
    "confidence": 0.9,
    "reasoning": "Text contains multiple positive words indicating strong positive sentiment"
  }
}
```

## Test Files

- `tests/sample_input_1.json`: Positive sentiment example
- `tests/sample_input_2.json`: Negative sentiment example
- `tests/expected_1.json`: Expected output for sample 1
- `tests/expected_2.json`: Expected output for sample 2

## Customization

### Modifying Extraction Criteria

Edit `extensions/text_extractor.yaml` to:
- Change the number of key points extracted
- Add new extraction fields
- Adjust extraction guidelines

### Modifying Sentiment Analysis

Edit `extensions/sentiment_analyzer.yaml` to:
- Change sentiment categories
- Adjust confidence calculation
- Modify reasoning format

### Adding Steps

To add more steps to the workflow:
1. Create a new extension in `extensions/`
2. Add a step to `agenticmodule.yaml` workflow
3. Map inputs from previous steps or module inputs

## Limitations

- Sentiment analysis depends on LLM interpretation
- Extraction quality varies with text complexity
- No feedback loop for accuracy improvement
- Sequential execution only (no parallel steps)

## Next Steps

- Review individual extensions to understand their behavior
- Test with your own text samples
- Customize extraction criteria for your needs
- Consider adding additional steps (e.g., keyword extraction, summarization)
