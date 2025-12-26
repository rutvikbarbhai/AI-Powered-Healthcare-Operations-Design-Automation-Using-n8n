Using the provided patient, procedure, and diagnosis information,
generate a complete insurance pre-authorization request.

Input will include:
- Patient demographics (anonymized)
- Insurance provider
- CPT procedure code
- ICD-10 diagnosis code
- Appointment urgency

Your task:
1. Determine medical necessity
2. Generate a concise clinical justification
3. Classify urgency (Routine / Urgent / Emergent)
4. Prepare structured output for portal submission

If information is insufficient:
- Set "confidence" to "low"
- Add "human_review_required": true

Use standard clinical terminology.
Do not include speculation or assumptions.
