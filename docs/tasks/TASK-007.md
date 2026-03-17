# TASK-007: Demo URDF

## Dependencies: TASK-001
## Wave: 2
## Files: `app/public/demo/robot.urdf`

## Instructions
Create a valid URDF XML file for a 6-DOF serial manipulator arm. Use ONLY inline geometry (cylinder, box) — no external mesh files.

Structure:
- **base_link**: cylinder r=0.05, length=0.1, gray (0.5,0.5,0.5). Fixed to world.
- **link_1 through link_6**: Each is a cylinder r=0.03, length=0.3. Colors: red(1,0.2,0.2), green(0.2,1,0.2), blue(0.2,0.2,1), yellow(1,1,0.2), cyan(0.2,1,1), magenta(1,0.2,1).
- **joint_1 through joint_6**: All revolute. Axes alternate: Z,Y,Y,Z,Y,Z. Limits: lower=-3.14159, upper=3.14159. Each joint origin is offset by 0.3m along the parent link's axis.
- Joint origins: joint_1 at z=0.1 (top of base), joints 2-6 each offset by length of previous link along its axis.

The URDF must be valid XML with proper `<robot>`, `<link>`, `<joint>`, `<visual>`, `<geometry>`, `<material>`, `<origin>` tags.

## Acceptance Criteria
- [ ] File is valid XML
- [ ] 7 links (base + 6), 6 revolute joints
- [ ] Each link has visual geometry with distinct color
