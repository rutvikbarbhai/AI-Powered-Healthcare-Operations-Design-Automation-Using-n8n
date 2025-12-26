

Review the generated pre-authorization request.

Check for:
- Missing required fields
- Unsupported clinical claims
- Inconsistent diagnosis/procedure pairing
- Overstated urgency

If any issue is detected:
- Flag "human_review_required" as true
- Provide a short validation_note explaining why

Do NOT rewrite the request.
Only validate and flag issues.
