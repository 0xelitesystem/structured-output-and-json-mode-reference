# Structured Output and JSON Mode

A plain-language reference on getting reliable structured output from language models: JSON mode, tool and function calling, schema enforcement, validation and retries, prompting for strict JSON, and the failure modes to expect. It is aimed at someone wiring a model into software that needs machine-readable output rather than free-form prose.

## The core idea

Structured output means getting a model to return data in a precise, machine-readable shape (usually JSON matching a schema) instead of free-form text, so your program can parse and use it reliably. The strongest approaches constrain the model: schema-enforced or constrained decoding guarantees valid structure, and tool or function calling has the model fill in defined arguments. Where the model can still produce invalid output, you defend with validation against a schema and retries. Prompting alone helps but is the weakest guarantee, so combine clear instructions with enforcement and validation.

## What is inside

- [01-why-structured-output.md](01-why-structured-output.md) the problem and the spectrum of approaches.
- [02-json-mode-and-schema-enforcement.md](02-json-mode-and-schema-enforcement.md) JSON mode and guaranteed-valid structure.
- [03-tool-and-function-calling.md](03-tool-and-function-calling.md) getting structure through defined function arguments.
- [04-validation-and-retries.md](04-validation-and-retries.md) checking output and recovering from bad output.
- [05-prompting-and-failure-modes.md](05-prompting-and-failure-modes.md) prompting for strict JSON and what goes wrong.

## The stance

This reference treats structured output as something you engineer with layered guarantees, not something you hope for from a well-worded prompt. The more you constrain and validate, the more reliable the result. The goal is output your code can trust, achieved by combining enforcement, validation, and sensible handling of the failures that remain.

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT. Copyright RBL Industries LLC 2026.
