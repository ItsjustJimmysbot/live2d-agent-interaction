# Frontend Brief

## Scope
Build `frontend-host` to render Live2D model and execute runtime commands.

## Deliverables
1. Live2D host page/app with model loader
2. Command handler for `motion`, `emotion`, `gaze`, `speech` event
3. Ack/error callback to backend ws bridge
4. Basic idle micro-animation loop

## Acceptance
- Connect to ws bridge and receive commands
- Execute at least 3 motions: idle/wave/nod
- Report success/failure for each command
- Handle fallback text-only mode without crash
