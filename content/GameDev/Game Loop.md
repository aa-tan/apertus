---
title: "Game Loop"
tags:
- gamedev
---
The Game Loop defines the individual steps a [[Game Engine|game engine]] executes in order to produce a working game.

There are three main steps that repeat as the game runs.

```mermaid
graph TD

	a(Input) --> b(Process)
	
	b --> c(Render)
	
	c --> a
 ```

## Input
User input can come in many forms. The first step of the game loop is typically to recieve input from a player that can then be processed by the engine.

Examples of potential inputs include:
- Button presses
- Mouse movement
- Gyro sensors like a phone tilting
- Sound activation from a microphone

## Process
Once the game engine receives input, it can now process or update the state of the game.

Examples of what can be updated:
- Object physics
- Enemy Health
- Player Experience

## Render
Now that game state has been updated, the final step is to display the new game state on the screen. After rendering, the game loop returns to the initial state of reading inputs and continues looping until the game terminates.

# Implementations
The game loop runs continuously as the driving force of the game engine. In modern computing however, machines come with different specifications that can affect performance in varying ways.

This performance variation can have an affect on the game loop depending on how fast or slow a machine runs. For example as video games are largely a visual medium, the render step within a gameloop is typically tied closely to the speed at which [[GameDev/Frame|frames]] are rendered. A game with logic tied directly to FPS may have enemies moving faster on a high-end machine compared to a low-end machine.

To handle this, there are two main methods of implementing a game loop, both with their own benefits and flaws. Deciding on which to use for your game engine depends on the type of game and target hardware being developed for.

## Fixed Time-step
asdf
## Variable Time-step
asdf