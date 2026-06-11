# JSON mode and schema enforcement

JSON mode is a setting that makes a model return syntactically valid JSON instead of free text, removing the risk of unparseable output. Schema enforcement goes further: it constrains the output to match a specific schema you provide, so the result has the exact fields and types you asked for. These features move structured output from "usually works" to "guaranteed valid."

## What JSON mode does

JSON mode instructs the model platform to emit only well-formed JSON. With it on, you do not get prose around the JSON, trailing commentary, or markdown code fences wrapping the object; you get a parseable JSON value. This solves the most basic failure: output you cannot even parse. What JSON mode alone does not guarantee is that the JSON has the structure you wanted; it ensures valid syntax, not the right fields. So it is a floor, not a complete solution.

## Schema enforcement

Schema enforcement (sometimes called structured outputs or constrained decoding) takes a schema, typically JSON Schema, and constrains generation so the output must conform to it. The model can only produce tokens that keep the output valid against the schema, so the required fields appear, the types are correct, and disallowed shapes cannot be generated. This is the strongest guarantee because validity is enforced during generation rather than checked afterward. When your platform supports it, schema enforcement is the most reliable way to get exactly the structure you need.

## How constrained decoding works

Under the hood, constrained decoding restricts the model's token choices at each step to only those that can still lead to a valid result for the given schema. If the next required thing is a closing brace or a specific field name, the model is limited to tokens consistent with that. The effect is that the output cannot drift out of the allowed structure, because the invalid options were never available. You do not need to implement this yourself; you just provide the schema and the platform enforces it.

## When enforcement is not available

Not every model or platform offers schema enforcement, and some offer only basic JSON mode. When full enforcement is unavailable, you fall back to a combination: turn on JSON mode if it exists, describe the desired schema clearly in the prompt, and then validate the parsed output against your schema in code, retrying on failure. This is less airtight than enforced decoding but, done carefully, is reliable enough for most uses. The principle stays the same: get as much guarantee from the system as you can, then validate the rest.
