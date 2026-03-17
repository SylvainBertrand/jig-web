# Rosbridge WebSocket Protocol Patterns

## Overview

rosbridge_suite provides a JSON-based WebSocket interface to ROS 2. No native ROS dependencies needed in the browser — just a WebSocket connection.

## Connection

```typescript
const ws = new WebSocket('ws://localhost:9090');

ws.onopen = () => console.log('Connected to rosbridge');
ws.onclose = () => console.log('Disconnected');
ws.onerror = (e) => console.error('WebSocket error:', e);
ws.onmessage = (e) => {
  const msg = JSON.parse(e.data);
  // Handle incoming messages
};
```

## Protocol Messages

### Get Topic List (via rosapi service)

```typescript
// Request
ws.send(JSON.stringify({
  op: 'call_service',
  service: '/rosapi/topics',
  args: {},
  id: 'get_topics'
}));

// Response
// { op: 'service_response', service: '/rosapi/topics', values: { topics: ['/joint_states', ...], types: ['sensor_msgs/msg/JointState', ...] }, id: 'get_topics', result: true }
```

### Subscribe to Topic

```typescript
ws.send(JSON.stringify({
  op: 'subscribe',
  topic: '/joint_states',
  type: 'sensor_msgs/msg/JointState',  // optional but recommended
  id: 'sub_joint_states'
}));

// Incoming messages arrive as:
// { op: 'publish', topic: '/joint_states', msg: { header: {...}, name: [...], position: [...], velocity: [...], effort: [...] } }
```

### Unsubscribe

```typescript
ws.send(JSON.stringify({
  op: 'unsubscribe',
  topic: '/joint_states',
  id: 'sub_joint_states'
}));
```

### Publish a Message

```typescript
ws.send(JSON.stringify({
  op: 'publish',
  topic: '/cmd_vel',
  msg: {
    linear: { x: 0.5, y: 0.0, z: 0.0 },
    angular: { x: 0.0, y: 0.0, z: 0.1 }
  }
}));
```

### Call a Service

```typescript
ws.send(JSON.stringify({
  op: 'call_service',
  service: '/some_service',
  args: { param1: 'value1' },
  id: 'call_1'
}));

// Response:
// { op: 'service_response', service: '/some_service', values: {...}, id: 'call_1', result: true }
```

## Client Class Pattern

```typescript
class RosbridgeClient {
  private ws: WebSocket | null = null;
  private topicCallbacks = new Map<string, (msg: any) => void>();
  private serviceCallbacks = new Map<string, (response: any) => void>();

  async connect(url: string): Promise<void> {
    return new Promise((resolve, reject) => {
      this.ws = new WebSocket(url);
      this.ws.onopen = () => resolve();
      this.ws.onerror = (e) => reject(e);
      this.ws.onmessage = (e) => this.handleMessage(JSON.parse(e.data));
    });
  }

  private handleMessage(msg: any) {
    if (msg.op === 'publish') {
      this.topicCallbacks.get(msg.topic)?.(msg.msg);
    } else if (msg.op === 'service_response') {
      this.serviceCallbacks.get(msg.id)?.(msg);
      this.serviceCallbacks.delete(msg.id);
    }
  }

  subscribe(topic: string, type: string, callback: (msg: any) => void) {
    this.topicCallbacks.set(topic, callback);
    this.ws?.send(JSON.stringify({ op: 'subscribe', topic, type }));
  }

  unsubscribe(topic: string) {
    this.topicCallbacks.delete(topic);
    this.ws?.send(JSON.stringify({ op: 'unsubscribe', topic }));
  }

  async getTopicList(): Promise<{ topics: string[]; types: string[] }> {
    const id = `topics_${Date.now()}`;
    return new Promise((resolve) => {
      this.serviceCallbacks.set(id, (resp) => resolve(resp.values));
      this.ws?.send(JSON.stringify({ op: 'call_service', service: '/rosapi/topics', args: {}, id }));
    });
  }

  disconnect() {
    this.ws?.close();
    this.ws = null;
    this.topicCallbacks.clear();
  }
}
```

## Important Notes

- rosbridge uses `bigint`-safe JSON for large timestamps — but in practice, JavaScript's `Number` handles ROS timestamps fine.
- The `type` field in subscribe is optional but helps rosbridge validate messages.
- Default rosbridge port is 9090.
- If rosbridge isn't running, the WebSocket connection will fail immediately — handle this gracefully in the UI.

## UNVERIFIED — Written from training knowledge. CC should cross-check against rosbridge_suite docs or .d.ts types if available.
