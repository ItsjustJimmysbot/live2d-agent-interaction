# Live2D Interaction Protocol v0.1

## 1) Inbound event (to orchestrator)

```json
{
  "traceId": "uuid",
  "timestamp": "ISO-8601",
  "type": "user_message|mention|idle_tick|system",
  "text": "string",
  "user": {
    "id": "string",
    "name": "string"
  },
  "channelMeta": {
    "platform": "discord|telegram|...",
    "chatId": "string"
  },
  "context": {
    "recentTurns": [],
    "agent": "arch-dev"
  }
}
```

## 2) Outbound action envelope (orchestrator -> bridge -> host)

```json
{
  "traceId": "uuid",
  "state": "listening|thinking|speaking|idle|error",
  "response": {
    "text": "string",
    "intent": "greet|explain|confirm|joke|other",
    "emotion": "neutral|happy|serious|surprised|thinking",
    "animation": {
      "motion": "idle|wave|nod|think",
      "expression": "neutral|smile|focused|surprised",
      "gaze": "camera|left|right|down",
      "priority": "low|normal|high"
    },
    "voice": {
      "enabled": false,
      "style": "default",
      "speed": 1.0,
      "pitch": 1.0
    }
  },
  "fallback": {
    "textOnly": true,
    "reason": "bridge_timeout|motion_unavailable|host_error|none"
  }
}
```

## 3) Host ack/error

```json
{
  "traceId": "uuid",
  "status": "ok|error",
  "phase": "received|executing|done",
  "error": {
    "code": "string",
    "message": "string"
  }
}
```

## 4) Validation rules

- `traceId` is required in all messages.
- `response.text` is required unless system noop event.
- Unknown `motion` must not hard-fail; switch to `fallback.textOnly=true`.
- Max end-to-end timeout per turn: 3500ms (configurable).

## 5) Versioning

- Current: `v0.1`.
- Backward compatible changes: add optional fields only.
- Breaking changes: bump to `v0.2+` and update bridge/host together.
