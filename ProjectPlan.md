# Procedural Racer - Project Plan

This document outlines the complete development roadmap for creating a multiplayer, procedurally generated racing game. The client will be hosted for free on the web, with matchmaking handled by a lightweight, free-tier backend service.

## Phase 1: The Core Gameplay Loop (Single-Player)

**Objective**: To create a fun and satisfying single-player driving experience on a static test track. If driving isn't fun by itself, the game has no foundation.

### ✅ To-Do Checklist

- [ ] Create a new Unity Project using the Universal Render Pipeline (URP).
- [ ] Import Kenney's Car Kit, Race Kit, and Nature Kit assets.
- [ ] Create a new scene and build a simple, static test track using ProBuilder and the imported assets. Include straights, turns, and a hill.
- [ ] Set up the player car prefab using a Rigidbody and four Wheel Colliders.
- [ ] Write the `CarController.cs` script to handle keyboard input and apply torque/steering to the Wheel Colliders.
- [ ] Tweak physics parameters (mass, center of mass, suspension, friction) until the car "feels" good to drive.
- [ ] Add a basic follow-camera script to track the car.
- [ ] Add basic sounds: engine noise that pitches with RPM, tire screeching on hard turns, and a simple impact sound.

### 🧪 Testing Checklist

- [ ] Can the car accelerate, brake, and reverse effectively?
- [ ] Does steering feel responsive but not overly twitchy?
- [ ] Can the car navigate the entire test track without getting stuck or flipping too easily?
- [ ] **The "Joy" Test**: Is it genuinely fun to just drive the car around the track for five minutes?
- [ ] Do the basic sounds play at the appropriate times?

### ⭐ Key Tips & Focus for Phase 1

- **Focus 90% on the Car "Feel"**: This is the soul of your game. Do not move on to Phase 2 until you are happy with how the car controls.
- **Iterate Rapidly**: Make small changes to physics values, hit play, test, stop, and repeat.
- **Use the Unity Profiler**: Keep an eye on performance from the start.

## Phase 2: Procedural Map Generation

**Objective**: To replace the static track with a new, random, complete, and drivable racetrack every time the game starts, based on a seed.

### ✅ To-Do Checklist

- [ ] Create a set of modular track prefabs (e.g., `Straight`, `Curve_Left`, `Curve_Right`). Ensure they all have consistent "entry" and "exit" connector points.
- [ ] Create a `TrackGenerator.cs` script.
- [ ] Implement the core logic of the **"Biased and Constrained Random Walk"** algorithm.
- [ ] Use a 2D grid system (`Dictionary<Vector2Int, GameObject>`) to track placed pieces and prevent overlaps.
- [ ] Implement the "first half" logic (wander away) and "second half" logic (bias towards the start).
- [ ] Implement the deterministic "closing the loop" logic for the final few pieces.
- [ ] Add a `public Generate(string seed)` method that uses the seed to control all `Random` calls.

### 🧪 Testing Checklist

- [ ] Does generating a track with the same seed multiple times produce the exact same layout?
- [ ] Do different seeds produce different, unique layouts?
- [ ] Generate 50+ tracks. Manually inspect them. Are there any gaps or overlapping pieces?
- [ ] Drive a dozen different generated tracks. Are they all drivable and fun?

### ⭐ Key Tips & Focus for Phase 2

- **Visualize Your Debugging**: Use `Debug.DrawRay` or `Gizmos` to draw your grid and connector points in the Scene view.
- **Start Simple**: Get `straight` and `curve` pieces working first. Then add biasing and loop closing.

## Phase 3: UI & Game State Management

**Objective**: To build the menus and a robust state machine that structures a race (start, race, finish).

### ✅ To-Do Checklist

- [ ] Create a master `GameManager.cs` script to act as a state machine (`MainMenu`, `Countdown`, `Racing`, `RaceOver`).
- [ ] Design and build the UI screens using UI Toolkit:
  - `MainMenu`: "Practice", "Multiplayer" buttons.
  - `HUD`: Speed, Lap Time, Race Position.
  - `RaceOverScreen`: "Winner: Player X", "Play Again" button.
- [ ] Implement the 3-2-1-GO! race countdown logic.
- [ ] Implement a simple lap tracking system using trigger colliders.
- [ ] Implement the in-game radio system with UI buttons.

### 🧪 Testing Checklist

- [ ] Can you navigate the entire game flow? (Menu -> Race -> End Screen -> Menu).
- [ ] Are car controls disabled during the countdown and after the race ends?
- [ ] Does the HUD update correctly in real-time?
- [ ] Is the lap counter accurate?
- [ ] Do the radio controls work as expected?

### ⭐ Key Tips & Focus for Phase 3

- **Centralize Logic**: The `GameManager` should be the "brain" of your game.
- **Separate UI from Logic**: UI scripts display data; the `GameManager` manages it.

## Phase 4: Peer-to-Peer Networking

**Objective**: To make the game multiplayer by allowing one player to host and others to join them directly via an IP address.

### ✅ To-Do Checklist

- [ ] Install the **Netcode for GameObjects** package.
- [ ] Add `NetworkObject` and `NetworkTransform` components to the car prefab.
- [ ] Convert key scripts like `GameManager` to `NetworkBehaviour`.
- [ ] Create a simple UI panel for hosting/joining with an IP input field.
- [ ] Implement `StartHost()` and `StartClient()` logic.
- [ ] Ensure the map generation seed is created by the host and sent to all clients.
- [ ] Use Client RPCs (`ClientRpc`) to synchronize game-wide events like the race start/end.

### 🧪 Testing Checklist

- [ ] Can a standalone build and the editor connect to each other on the same PC (`127.0.0.1`)?
- [ ] Test with a second computer on the same local network.
- [ ] Does player movement synchronize between clients?
- [ ] Do all players generate the *exact same map* from the synced seed?
- [ ] Does the race start and end for everyone at the same time?

### ⭐ Key Tips & Focus for Phase 4

- **Test in a Build**: The Unity Editor can behave differently than a real build. Always test with standalone builds.
- **Host Authority**: The host should be the authority on the game state (race timer, map seed).

## Phase 5: LAN Discovery & Final Deployment

**Objective**: To allow players on the same local network to automatically find and join games without manually entering IP addresses, and to package and deploy the game for players.

### ✅ To-Do Checklist

- [ ] Implement a **LAN Discovery** system. This involves:
  - **Host side**: The host's game will send out UDP broadcast messages every second, announcing its presence on the network.
  - **Client side**: The joinee's game will listen for these UDP broadcasts.
- [ ] Create a "Join Game" UI that displays a list of all discovered games on the local network.
- [ ] Implement the logic so that clicking a game in the list automatically connects the client to that host.
- [ ] Build the final Unity project for **WebGL**.
- [ ] Enable **GitHub Pages** and upload the WebGL build so people can play instantly in their browser.
- [ ] Update the final `README.md` with links to the GitHub Pages game and the Releases page.

### 🧪 Testing Checklist

- [ ] On a local network, does a game hosted by one player appear in the "Join Game" list for other players?
- [ ] Can players successfully join the game from the discovered list?
- [ ] Does the discovery list correctly update when a host starts or stops a game?
- [ ] Is the GitHub Pages link working?

### ⭐ Key Tips & Focus for Phase 5

- **Use a Library for Discovery**: Networking libraries like **Mirror** have a `NetworkDiscovery` component that handles UDP broadcasting for you. For Netcode for GameObjects, you may need to write this logic yourself, but it's a well-documented process.
- **Clear Instructions**: Your `README.md` should clearly explain that to host a game securely (with secret keys), players should use the downloadable Standalone build.
