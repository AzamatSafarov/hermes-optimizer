# Auxiliary config shape

Hermes has a fixed set of auxiliary task slots. Each slot takes a provider and a model, and may
carry its own endpoint and its own fallback chain.

## The canonical shape

Write nested per-task dicts under `auxiliary`. The key is the task slot name, verbatim.

```yaml
auxiliary:
  vision:
    provider: azure-foundry
    model: gpt-5.4
  web_extract:
    provider: ollama-cloud
    model: kimi-k2.5
  compression:
    provider: ollama-cloud
    model: kimi-k2.5
  approval:
    provider: ollama-cloud
    model: deepseek-v4-flash
  skills_hub:
    provider: ollama-cloud
    model: gpt-oss:20b
```

## Canonical slot names

Use exactly these keys. Anything else is a shape the Desktop app will not render.

`vision`, `web_extract`, `compression`, `skills_hub`, `approval`, `mcp`, `title_generation`,
`triage_specifier`, `kanban_decomposer`, `profile_describer`, `curator`, `review`,
`background_review`, `goal_judge`, `memory_query_rewrite`, `tts_audio_tags`, `monitor`,
`moa_reference`, `moa_aggregator`.

## What not to do

Do **not** invent umbrella keys such as `heavy`, `medium`, or `light` under `auxiliary`:

```yaml
# WRONG - the Desktop Models page does not recognize these
auxiliary:
  heavy:
    provider: ollama-cloud
    model: kimi-k2.5
```

If you write this, the YAML changes but the app keeps showing the old settings, and the user
concludes the optimization did not work. The lanes are a **thinking tool** for choosing models,
not a config construct. Map each lane onto the real slot names before writing.

## Custom and local endpoints

For a custom or local endpoint, the endpoint belongs on the task itself, not only on the main
model. Otherwise the auxiliary slot inherits the wrong base URL.

```yaml
auxiliary:
  compression:
    provider: custom
    model: my-model
    base_url: http://127.0.0.1:1234/v1
    api_key: dummy
```

## Per-slot fallback chains

A slot can carry its own fallback chain. This is worth using when a slot is served by a provider
that is less reliable than the main one.

```yaml
auxiliary:
  compression:
    provider: ollama-cloud
    model: glm-5.3-flash
    fallback_chain:
      - provider: azure-foundry
        model: deepseek-v4.1-flash
        base_url: https://example.cognitiveservices.azure.com/openai/v1
        api_mode: chat_completions
```

Note the `api_mode` on the fallback entry. A provider that speaks the responses API needs
`api_mode: responses`; a chat-completions endpoint needs `api_mode: chat_completions`. Getting
this wrong produces a route that looks configured and fails on the first call.

## Main fallback chain

The main-model fallback chain is a separate top-level key, not under `auxiliary`:

```yaml
fallback_providers:
  - provider: azure-foundry
    model: deepseek-v4.1-flash
    base_url: https://example.cognitiveservices.azure.com/openai/v1
    api_mode: chat_completions
```

A fallback that resolves to the same provider and endpoint as the main route is decoration. If
the primary fails because that provider is down, the fallback fails the same way. Point the
fallback at a genuinely different route when the user has one.

## Verification

After writing any of this:

1. Re-read the config file and confirm the keys landed as written.
2. Check the Desktop page reflects the change. If the app shows the old value, the shape is wrong.
3. Smoke-test at least one slot by triggering it or by a direct call, so you know the route is
   live rather than merely configured.