# Backend Brief

## Scope
Build `realtime-bridge` for orchestrator <-> frontend-live2d communication.

## Deliverables
1. WebSocket server (`/ws`) for action dispatch and ack
2. REST endpoint (`POST /event`) receiving user event payload
3. Action queue with simple state machine (idle/listening/thinking/speaking/error)
4. Fallback policy when motion dispatch fails

## Acceptance
- Receive mock user event and emit structured action to ws client
- Receive ws ack and update state
- Errors are logged and fallback text-only action is emitted
