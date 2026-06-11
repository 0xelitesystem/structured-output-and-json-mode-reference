# Tool and function calling

Tool calling (also called function calling) is a way to get structured output by defining a function with a typed parameter schema and having the model produce arguments that fit it. Instead of asking for JSON in prose, you give the model a function signature, and it returns a structured call with values for each parameter, which the platform validates against the schema.

## How it works

You declare one or more functions, each with a name, a description, and a schema for its parameters (usually JSON Schema). You send these declarations along with the user's request. The model, when it decides the function applies, returns a structured response indicating which function to call and a set of arguments matching the parameter schema. Your code receives those arguments as structured data, runs whatever the function actually does, and can return a result to the model. The structured arguments are the structured output.

## Structure as a side effect

A useful insight is that tool calling gives you structured output even when you have no real function to run. If you just want the model to extract fields into a fixed shape, you can define a function whose parameters are exactly that shape, ask the model to call it, and take the arguments as your result, never executing anything. The function declaration becomes a schema specification in disguise. Many structured-extraction tasks are implemented this way, using the function-calling mechanism purely to pin down the output format.

## Multiple tools and choice

When you provide several functions, the model selects which one (if any) fits the request, and some platforms let it call more than one. This is what lets a model act as an agent: it chooses the appropriate tool for the situation. For pure structured output you usually offer a single function and want it always called, which platforms often let you force. The distinction is whether you want the model to decide among options (agentic use) or always produce one fixed structure (extraction use).

## Strengths and limits

Tool calling is well supported and gives strong structure because arguments are validated against the parameter schema, so you tend to get correctly shaped data. Its limits are that the model still chooses the values, so the contents can be wrong even when the shape is right, and that very complex or deeply nested schemas can strain reliability. As always, validate the returned arguments against your real requirements, including value-level rules the schema cannot express, rather than assuming correct shape means correct content.
