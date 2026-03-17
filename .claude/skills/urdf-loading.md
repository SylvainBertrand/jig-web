# URDF Loading with urdf-loader

## Installation
```bash
npm install urdf-loader
```

## Loading a URDF in React Three Fiber

```typescript
import URDFLoader from 'urdf-loader';
import { useLoader } from '@react-three/fiber';
import * as THREE from 'three';

// Option 1: Imperative loading (recommended for flexibility)
function RobotModel({ urdfPath, sourceId }: Props) {
  const [robot, setRobot] = useState<any>(null);
  const groupRef = useRef<THREE.Group>(null);

  useEffect(() => {
    const loader = new URDFLoader();
    // Set the base path for mesh files referenced by the URDF
    loader.packages = (pkg: string) => '/demo/';  // maps package:// to /demo/

    loader.load(urdfPath, (result) => {
      setRobot(result);
      if (groupRef.current) {
        groupRef.current.add(result);
      }
    });

    return () => {
      if (groupRef.current && robot) {
        groupRef.current.remove(robot);
      }
    };
  }, [urdfPath]);

  return <group ref={groupRef} />;
}
```

## Setting Joint Values

```typescript
// The loaded robot has a `joints` property — a Map<string, URDFJoint>
// URDFJoint extends THREE.Object3D and has setJointValue(value)

function updateJoints(robot: any, jointValues: Map<string, number>) {
  if (!robot || !robot.joints) return;

  for (const [jointName, joint] of Object.entries(robot.joints)) {
    const value = jointValues.get(jointName);
    if (value !== undefined) {
      (joint as any).setJointValue(value);
    }
  }
}

// Joint names come from the URDF <joint name="..."> tags
// For demo URDF with joints named joint_1..joint_6:
const jointNames = Object.keys(robot.joints); // ['joint_1', 'joint_2', ...]
```

## Iterating Links (for Frame Axes)

```typescript
function getLinks(robot: any): { name: string; worldPosition: THREE.Vector3; worldQuaternion: THREE.Quaternion }[] {
  const links: any[] = [];
  robot.traverse((child: THREE.Object3D) => {
    if ((child as any).isURDFLink) {
      const pos = new THREE.Vector3();
      const quat = new THREE.Quaternion();
      child.getWorldPosition(pos);
      child.getWorldQuaternion(quat);
      links.push({ name: child.name, worldPosition: pos, worldQuaternion: quat });
    }
  });
  return links;
}
```

## Appearance Override (Color, Opacity)

```typescript
function setRobotAppearance(robot: any, color?: string, opacity: number = 1.0) {
  robot.traverse((child: THREE.Object3D) => {
    if (child instanceof THREE.Mesh) {
      const material = child.material as THREE.MeshStandardMaterial;
      if (color) {
        material.color.set(color);
      }
      material.opacity = opacity;
      material.transparent = opacity < 1.0;
    }
  });
}
```

## URDF with Inline Geometry (No External Meshes)

For the demo robot, use URDF with `<cylinder>` and `<box>` primitives — no external STL/DAE files needed. urdf-loader handles these natively:

```xml
<visual>
  <geometry>
    <cylinder radius="0.03" length="0.3"/>
  </geometry>
  <material name="red">
    <color rgba="1.0 0.2 0.2 1.0"/>
  </material>
</visual>
```

## Binary Search for Joint Values at Time t

```typescript
function getValueAtTime(series: ScalarSeries, t: number): number {
  const { timestamps, values } = series;
  if (timestamps.length === 0) return 0;
  if (t <= timestamps[0]) return values[0];
  if (t >= timestamps[timestamps.length - 1]) return values[timestamps.length - 1];

  // Binary search for the index where timestamps[idx] <= t < timestamps[idx+1]
  let lo = 0, hi = timestamps.length - 1;
  while (lo < hi) {
    const mid = (lo + hi + 1) >> 1;
    if (timestamps[mid] <= t) lo = mid;
    else hi = mid - 1;
  }
  return values[lo];
}
```

## Verified against: https://www.npmjs.com/package/urdf-loader on 2026-03-16
