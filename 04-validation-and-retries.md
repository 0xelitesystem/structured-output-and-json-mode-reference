# Validation and retries

Even with JSON mode or schema enforcement, you should validate the model's output against your schema in code before trusting it, and have a retry path for when validation fails. Validation catches structural and value problems; retries give the model a chance to fix its own mistake. Together they turn occasional bad output into a recoverable, rather than fatal, event.

## Always validate

Parse the output and check it against your schema in your own code, regardless of what enforcement the platform claims to provide. Validation confirms the required fields are present, the types are right, and value-level rules hold, such as a number being in range or a string matching an expected set. Schema enforcement guarantees shape but not that the values make sense; prompting and JSON mode guarantee even less. Validating in code is the backstop that ensures your program only acts on output it has actually verified, never on assumptions.

## Retrying on failure

When validation fails, the common recovery is to retry. The most effective retry sends the model its invalid output along with the specific error, asking it to correct that problem, because a targeted "this field was missing" prompt fixes things faster than a blind re-ask. Set a small retry limit so a persistently failing case does not loop forever. Many transient formatting errors disappear on the first corrective retry, which is why this simple loop substantially improves reliability without much code.

## Targeted error feedback

The quality of the retry depends on the feedback you give. Returning the exact validation error (the missing field, the wrong type, the out-of-range value) lets the model address the actual defect instead of guessing. Generic "that was wrong, try again" prompts are far less effective. Treat the retry like a code review comment: be specific about what failed and what is expected. This turns the model's second attempt from a coin flip into a focused correction, which is what makes retries reliable rather than hopeful.

## When to stop retrying

Retries should be bounded and have a fallback. After a few failed attempts, stop and handle the failure deliberately: surface an error, fall back to a default, route to a human, or log it for review. Endless retrying wastes time and money and can mask a real problem, such as a request the model genuinely cannot satisfy or a schema that is too strict. Knowing when to give up gracefully is as important as retrying, because a structured-output pipeline needs a defined behavior for the cases that simply will not validate.
