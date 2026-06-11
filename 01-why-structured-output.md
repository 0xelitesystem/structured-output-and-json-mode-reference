# Why structured output

Structured output is when a model returns data in a fixed, machine-readable format, almost always JSON conforming to a known shape, so that software can parse and act on it directly. The problem it solves is that models naturally produce free-form text, which is fine for humans but unreliable for programs that need specific fields in a specific format every time.

## The problem

If you ask a model a question and it answers in prose, your code has to parse that prose to extract the answer, which is fragile: the wording varies, the model adds commentary, and small phrasing changes break your parser. When a model's output feeds another system (a database write, an API call, a UI), you need it in a predictable structure with the right fields and types. Free-form text cannot give that guarantee. Structured output exists to make the model's response something a program can consume safely.

## The spectrum of approaches

There is a range of techniques, from weakest to strongest guarantee. At the weak end, you simply ask the model in the prompt to return JSON and hope it complies. Stronger, JSON mode forces the output to be syntactically valid JSON. Stronger still, schema enforcement or constrained decoding makes the output conform to a specific schema, not just valid JSON. Tool and function calling has the model produce arguments matching a defined signature. Each step trades a little flexibility for a lot more reliability.

## Why constraints beat hope

Prompting alone relies on the model choosing to follow instructions, which it usually does but not always, so it is the weakest guarantee. Enforcement mechanisms change the situation: instead of asking the model to behave, they make malformed output impossible or check it before you trust it. The practical lesson is to push as far toward enforcement as your platform allows, because a guarantee enforced by the system is worth far more than an instruction the model may ignore on the input that matters most.

## Layering for reliability

The most robust setups combine layers rather than relying on any single one. You use enforcement (JSON mode, a schema, or function calling) to make valid output the default, you validate the result against your schema in code to catch anything that slips through, and you have a retry or fallback path for when validation fails. No single layer is perfect, but together they make structured output dependable enough to build on. The rest of this reference covers each layer in turn.
