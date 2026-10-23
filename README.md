# Model catalog

One file, `models.json`: for each language model, what it accepts. It is maintained by whoever manages the provider integrations; applications read it to prefill a model's configuration at registration, so the operator corrects instead of typing from scratch.

It is a bridge: until the platform distributes a catalog through its services, this file is the source.

## What each entry says

| Field | Meaning |
|---|---|
| `provider` | who makes the model: `anthropic`, `openai`, `gemini`, `mistral` |
| `model_id` | the name sent to the provider |
| `aliases` | other names the provider accepts for the same model |
| `context_window` | context tokens; `null` if not measured |
| `max_output_tokens` | maximum output tokens; `null` if not measured |
| `reasoning_levels` | the reasoning levels the model accepts, on the single scale `none < minimal < low < medium < high < xhigh < max`; empty if it does not reason |
| `reasoning_default` | the level applied when none is given |
| `reasoning_off` | how the `none` level is sent when it is not an ordinary value of the parameter (`between_tools`, `disabled`); `null` otherwise |
| `temperature_policy` | when temperature can be sent: `always`, `when_reasoning_off`, `never` |
| `temperature_recommended` | the value recommended by the provider, if it gives one |
| `reads_pdf` | whether it reads a PDF on its own |
| `notes` | how the value was measured, and exceptions |

The schema in `schema.json` fixes the shape and the allowed values. Readers of the file validate it before using it.

## Rules

- No keys, no endpoints, no tenant data: only model capabilities.
- An unmeasured value is `null`, and the note says why. No values guessed from the model name.
- Every change updates `updated_at`. A change of shape bumps `version` and updates `schema.json`.
- Readers of the file pin to a tag or a commit, not to the main branch.
