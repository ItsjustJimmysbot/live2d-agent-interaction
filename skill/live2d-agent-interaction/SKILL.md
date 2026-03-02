---
name: live2d-agent-interaction
description: Enable an OpenClaw agent to orchestrate Live2D interactive behavior with users. Use when building or operating flows that convert user/chat events into structured character actions (motion, expression, gaze, speech), synchronize text/voice output, and route commands through a runtime bridge to a Live2D host with fallback/error recovery.
---

# Live2D Agent Interaction

## Execute in 5 steps

1. Normalize incoming event into protocol shape (`type`, `text`, `context`, `channelMeta`).
2. Infer response intent/emotion and generate one structured action envelope.
3. Send envelope to bridge transport (WebSocket or HTTP relay).
4. Wait for host ack/error and update interaction state.
5. On error/timeout, emit fallback `textOnly` response.

## Required contract

Always produce a single JSON action envelope matching `references/protocol.md`:
- `traceId`
- `state`
- `response.text`
- `response.animation`
- `response.voice`
- `fallback`

Never emit SDK-specific calls from the orchestrator. Keep SDK binding inside runtime/host.

## State policy

Use only:
- `idle`
- `listening`
- `thinking`
- `speaking`
- `error`

Allowed transitions:
- `idle -> listening -> thinking -> speaking -> idle`
- any state -> `error` -> `idle`

## Fallback policy

Trigger fallback when:
- bridge timeout
- host command rejection
- missing animation mapping

Fallback behavior:
1. Keep text reply
2. Disable animation for this turn
3. Disable voice if transport unstable
4. Return to `idle`

## Resource loading guide

- Read `references/protocol.md` before implementing bridge/host wiring.
- Read `references/motion-map.md` when tuning intent-to-motion mapping.
- Read `references/state-machine.md` when debugging transition conflicts.

## Implementation note

Prefer deterministic adapters in `scripts/` for:
- event normalization
- schema validation
- bridge payload dispatch

Keep model/persona prompts in `assets/templates/`.
