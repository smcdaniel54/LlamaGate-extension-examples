# ROI Assessment Minimal Extension

This extension calculates Return on Investment (ROI) from financial inputs and provides a decision recommendation based on ROI percentage and risk factors.

## What It Does

Takes financial data (initial cost, annual savings, risk factor, time horizon) and:
- Calculates total savings over the time horizon
- Computes net ROI and ROI percentage
- Applies risk adjustment to ROI
- Provides a decision recommendation (strong_approve, approve, review, or reject)

## Inputs

- **initial_cost** (number, required): Initial investment or cost (must be >= 0)
- **annual_savings** (number, required): Expected annual savings or revenue (must be >= 0)
- **risk_factor** (number, required): Risk factor from 0.0 (low risk) to 1.0 (high risk)
- **time_horizon_years** (number, optional): Time horizon for ROI calculation in years (default: 3, minimum: 1)

## Outputs

- **total_savings** (number): Total savings over the time horizon
- **net_roi** (number): Net ROI (total savings minus initial cost)
- **roi_percentage** (number): ROI as a percentage
- **risk_adjusted_roi** (number): ROI adjusted for risk factor
- **recommendation** (string): One of "strong_approve", "approve", "review", or "reject"
- **reasoning** (string): Brief explanation of the recommendation

## How to Run

1. Ensure LlamaGate is running with access to a local LLM
2. Place this extension in your LlamaGate extensions directory
3. Provide input in the format shown in `test_input.json`
4. Execute the extension via LlamaGate API, CLI, or interface
5. Review the ROI calculations and recommendation

## Configuration

### Model Settings

The extension uses:
- **Provider:** Ollama (change if using a different provider)
- **Model:** llama3.2 (update to match your available model)
- **Temperature:** 0.2 (low for consistent calculations)

### Customization

To adjust behavior:
- Modify the recommendation thresholds in the prompt
- Change the risk adjustment formula if needed
- Add additional financial metrics (e.g., payback period, NPV)

## What "Good Output" Looks Like

A successful output should:
- ✅ Contain all required fields with correct data types
- ✅ Have accurate calculations (verify manually if needed)
- ✅ Include a valid recommendation from the enum
- ✅ Provide clear reasoning for the recommendation
- ✅ Be valid JSON that can be parsed

### Example Good Output

```json
{
  "total_savings": 150000,
  "net_roi": 50000,
  "roi_percentage": 50.0,
  "risk_adjusted_roi": 25000,
  "recommendation": "approve",
  "reasoning": "Positive risk-adjusted ROI and ROI percentage above 50% threshold, indicating a solid investment opportunity with acceptable risk."
}
```

## Decision Logic

The recommendation is based on:
- **Strong Approve:** Risk Adjusted ROI > 50% of initial cost AND ROI Percentage > 100%
- **Approve:** Risk Adjusted ROI > 0 AND ROI Percentage > 50%
- **Review:** Risk Adjusted ROI > 0 OR ROI Percentage > 0
- **Reject:** Risk Adjusted ROI <= 0 AND ROI Percentage <= 0

## Test Files

- `test_input.json`: Sample financial input data
- `expected_output.json`: Expected output structure (values may vary slightly based on model calculations)

## Limitations

- Calculations are performed by the LLM and should be verified for critical decisions
- Risk adjustment is simplified (multiplies by (1 - risk_factor))
- Does not account for inflation, discount rates, or other financial complexities
- Recommendation logic is basic and may need customization for your use case
- For production use, consider implementing actual calculation logic rather than relying on LLM math
