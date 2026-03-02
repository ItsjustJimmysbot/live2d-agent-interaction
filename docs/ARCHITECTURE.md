# Live2D Agent Interaction - Architecture v0

## Goal
Enable one OpenClaw agent to control a Live2D character and interact with users through text/action/expression/voice.

## System Layers
1. Input Layer
   - Discord/user message events
   - mention/idle/system events
2. Orchestrator (agent)
   - intent + emotion inference
   - output structured action schema
3. Runtime Bridge (backend)
   - websocket/http bridge
   - action queue + state machine + fallback
4. Frontend Live2D Host (front)
   - Live2D model render
   - motion/expression/lip-sync execution
5. Feedback Layer
   - ack/error metrics back to orchestrator

## Output Schema (draft)
```json
{
  "text": "string",
  "intent": "greet|explain|joke|confirm|other",
  "emotion": "neutral|happy|serious|surprised",
  "motion": "idle|wave|nod|think",
  "tts": {"enabled": false, "voice": "default", "speed": 1.0},
  "fallback": {"textOnly": true}
}
```

## Repos/Modules in this workspace
- `skill/live2d-agent-interaction/`: skill package content
- `realtime-bridge/`: backend bridge service (ws/api/state)
- `frontend-host/`: live2d web host/controller

## Milestones
- M1: schema + bridge skeleton + host skeleton
- M2: end-to-end text + 3 motions
- M3: tts/lipsync + state machine + retries
- M4: packaging and docs
