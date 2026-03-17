# Three.js + @react-three/fiber Patterns

## Setup

```typescript
import { Canvas, useFrame, useThree } from '@react-three/fiber';
import { OrbitControls, Html, Grid } from '@react-three/drei';
import * as THREE from 'three';
```

## Canvas in a Panel

```tsx
// Canvas MUST have explicit dimensions. In a FlexLayout panel, use flex + absolute positioning:
<div style={{ width: '100%', height: '100%', position: 'relative' }}>
  <Canvas
    camera={{ position: [3, 3, 3], up: [0, 0, 1] }} // Z-up!
    gl={{ antialias: true }}
    style={{ position: 'absolute', inset: 0 }}
  >
    <SceneSetup />
    <RobotModel />
  </Canvas>
</div>
```

## Z-Up Convention (Robotics)

Three.js defaults to Y-up. For Z-up robotics convention:

```tsx
// Set camera up vector to Z
<Canvas camera={{ position: [3, 3, 3], up: [0, 0, 1] }}>

// Grid on XY plane (ground plane for Z-up)
function GroundGrid() {
  return (
    <gridHelper
      args={[20, 20, '#363a4a', '#2a2d3a']}
      rotation={[Math.PI / 2, 0, 0]}  // rotate Y-up grid to XY plane
    />
  );
}
```

## OrbitControls

```tsx
import { OrbitControls } from '@react-three/drei';

<OrbitControls
  makeDefault
  enableDamping
  dampingFactor={0.1}
  target={[0, 0, 0]}
  up={[0, 0, 1]}  // Z-up
/>
```

## Camera Tracking (Follow a Link)

```tsx
function CameraTracker({ targetPosition }: { targetPosition: THREE.Vector3 | null }) {
  const { controls } = useThree();

  useFrame(() => {
    if (!targetPosition || !controls) return;
    const orbitControls = controls as any; // OrbitControls type
    const current = orbitControls.target as THREE.Vector3;
    current.lerp(targetPosition, 0.1); // smooth follow
    orbitControls.update();
  });

  return null;
}
```

## Lights Setup

```tsx
function Lights() {
  return (
    <>
      <ambientLight intensity={0.4} />
      <directionalLight position={[5, 10, 5]} intensity={0.8} castShadow />
    </>
  );
}
```

## Coordinate Frame Axes (XYZ Triads)

```tsx
function FrameAxes({ position, quaternion, size = 0.15 }: Props) {
  return (
    <group position={position} quaternion={quaternion}>
      <arrowHelper args={[new THREE.Vector3(1,0,0), new THREE.Vector3(), size, 0xff0000, size*0.2, size*0.1]} />
      <arrowHelper args={[new THREE.Vector3(0,1,0), new THREE.Vector3(), size, 0x00ff00, size*0.2, size*0.1]} />
      <arrowHelper args={[new THREE.Vector3(0,0,1), new THREE.Vector3(), size, 0x0000ff, size*0.2, size*0.1]} />
    </group>
  );
}
```

## Html Overlays (Labels in 3D)

```tsx
import { Html } from '@react-three/drei';

function Label3D({ position, text }: { position: [number,number,number]; text: string }) {
  return (
    <Html position={position} center distanceFactor={8}>
      <div className="bg-surface/80 text-xs px-1 rounded text-textPrimary whitespace-nowrap">
        {text}
      </div>
    </Html>
  );
}
```

## useFrame for Per-Frame Updates

```tsx
// useFrame runs every render frame (~60fps)
function AnimatedRobot({ robot, getJointValue }) {
  useFrame(() => {
    if (!robot) return;
    robot.joints.forEach((joint, name) => {
      const value = getJointValue(name);
      if (value !== undefined) {
        joint.setJointValue(value);
      }
    });
  });
  return null;
}
```

## Imperative Three.js Access via Refs

```tsx
function CustomMesh() {
  const meshRef = useRef<THREE.Mesh>(null);

  useFrame((state, delta) => {
    if (meshRef.current) {
      meshRef.current.rotation.z += delta * 0.5;
    }
  });

  return (
    <mesh ref={meshRef}>
      <boxGeometry args={[1, 1, 1]} />
      <meshStandardMaterial color="orange" />
    </mesh>
  );
}
```

## Cleanup on Unmount

```tsx
useEffect(() => {
  return () => {
    // Dispose geometries and materials to prevent memory leaks
    scene.traverse((child) => {
      if (child instanceof THREE.Mesh) {
        child.geometry.dispose();
        if (child.material instanceof THREE.Material) {
          child.material.dispose();
        }
      }
    });
  };
}, []);
```

## Environment Objects (Primitives)

```tsx
function EnvironmentObject({ obj }: { obj: EnvironmentObject }) {
  switch (obj.type) {
    case 'box':
      return (
        <mesh position={obj.position}>
          <boxGeometry args={obj.size} />
          <meshStandardMaterial color={obj.color} />
        </mesh>
      );
    case 'sphere':
      return (
        <mesh position={obj.position}>
          <sphereGeometry args={[obj.radius, 32, 32]} />
          <meshStandardMaterial color={obj.color} />
        </mesh>
      );
    case 'cylinder':
      return (
        <mesh position={obj.position}>
          <cylinderGeometry args={[obj.radius, obj.radius, obj.height, 32]} />
          <meshStandardMaterial color={obj.color} />
        </mesh>
      );
    case 'plane':
      return (
        <mesh position={obj.position} rotation={[-Math.PI/2, 0, 0]}>
          <planeGeometry args={obj.size} />
          <meshStandardMaterial color={obj.color} side={THREE.DoubleSide} />
        </mesh>
      );
  }
}
```

## Verified against: @react-three/fiber and @react-three/drei npm docs (general patterns). Specific drei component APIs should be cross-checked against .d.ts types.
