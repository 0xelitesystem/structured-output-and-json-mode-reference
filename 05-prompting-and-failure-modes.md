# Prompting and failure modes

Even when you use enforcement, clear prompting improves structured output: state the exact format, show an example, and tell the model to return only the data. And it helps to know the common failure modes (extra prose, markdown fences, wrong types, hallucinated fields, truncation) so you can prevent or detect them. Good prompting reduces failures; awareness of failure modes lets you handle the rest.

## Prompting for strict JSON

Describe the desired output precisely: the exact fields, their types, and an example of a valid response. Tell the model to return only the JSON with no explanation, no preamble, and no markdown code fences, since those are frequent contaminants. Giving a concrete example of the target shape is often more effective than describing it in words. Even with JSON mode or schema enforcement active, a clear prompt steers the values and reduces the chance the model fights the format, so the two work best together.

## Common failure modes

The recurring failures are predictable. The model wraps JSON in markdown fences or adds a sentence of commentary, breaking naive parsers. It returns the wrong type, a string where you expected a number, or omits a required field. It invents fields you did not ask for, or fills a field with a plausible but fabricated value. On long outputs it can be truncated by a token limit, producing valid-looking but incomplete JSON. Each of these is common enough that a robust pipeline should expect and check for it.

## Preventing them

Many failures have direct preventions. JSON mode or schema enforcement eliminates fences and most syntax and type errors. An explicit instruction to return only JSON cuts the commentary. A clear schema with required fields and an example reduces missing and invented fields. Raising the output token limit and asking for concise values prevents truncation. Keeping the requested structure as simple as the task allows reduces every category of error, because complex nested schemas are harder for the model to satisfy reliably.

## Detecting and handling the rest

What you cannot prevent, you detect. Strip or reject stray fences and prose before parsing. Validate types and required fields, and on failure, retry with the specific error. Watch for truncation by checking the JSON actually closes and parses completely. For fabricated values, validate against external facts where you can, since the model can produce a well-formed field with a wrong value that no schema will catch. The combination of preventing what you can and detecting what you cannot is what makes structured output trustworthy in practice, rather than merely usually correct.
