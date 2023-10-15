---
title: "Game Physics"
tags:
- gamedev
- physics
---
# Movement
### Speed
The rate of change of position over a fixed time-step in any direction.
>Speed = Distance / Time

### Acceleration
The change of speed over a given time.
>Acceleration = Δ Speed / Δ Time

### Velocity
The speed of an object in a given direction. Denoted as a [[GameDev/Vector|Vector]]. Implementation of velocity in game [[GameDev/Kinematics|Kinematics]] largely depends on the chosen [[GameDev/Game Loop|Game Loop]].

#### [[GameDev/Game Loop#Fixed Time-step|Fixed Time-step]]
Take the current position of an object and apply a vector to change that position on every [[GameDev/Frame|Frame]].

#### [[GameDev/Game Loop#Variable Time-step|Variable Time-step]]
Take the current position of an object and apply a vector to change that position. Multiply by vector time.