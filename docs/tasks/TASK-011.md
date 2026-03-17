# TASK-011: Rosbridge Client [SHELL]

## Dependencies: TASK-002
## Wave: 3
## Files: `app/src/core/rosbridgeClient.ts`
## Skills: `rosbridge-patterns.md`

## Instructions
Export class `RosbridgeClient` with: `connect(url): Promise<void>`, `disconnect()`, `getTopicList(): Promise<RosTopicInfo[]>` (calls /rosapi/topics), `subscribe(topic, type, callback)`, `unsubscribe(topic)`, `publish(topic, type, msg)`, `callService(service, args): Promise<any>`. Uses WebSocket with JSON rosbridge protocol. Store callbacks in a Map. Handle reconnection gracefully. All methods must be implemented — this is [SHELL] because we can't test without a real robot, but the code must be complete.

## Acceptance Criteria
- [ ] All methods implemented with correct rosbridge JSON protocol
- [ ] Compiles clean
