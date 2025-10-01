# The Chaos Circuit 🏎️💨

An open-source, multiplayer racing game built with Unity, featuring new, randomly generated tracks every time you play.

## ✨ Features

- **Infinite Racetracks**: Never drive the same track twice thanks to procedural generation.
- **Online Local Multiplayer**: Race against friends or in real-time over same network.
- **Cross-Platform**: Play instantly in your browser (WebGL) or on your Android device.
- **Arcade Physics**: Easy to learn, fun to master car handling.

---

## 🎮 Play & Download

You can play the latest version instantly in your browser or download the APK for Android from the official releases page.

- Play In Browser: Link to be updated
- Download for Android: Link to be updated

---

## 🤔 How the Maps are being generated ?

The racetracks in this game aren't pre-built; they are generated on the fly using a procedural algorithm. This ensures that every race can be a new and unique experience.

The process works by intelligently stitching together a library of pre-made, modular track pieces (like straights and curves) on a grid. An algorithm called the **"Biased Random Walk"** guides the track's creation, preventing overlaps and ensuring it always loops back to the start line to form a complete circuit.

### How a "Seed" Creates Identical Tracks

The key to making this work for multiplayer is the **seed**. A seed is a number that acts as the starting point for the random number generator, ensuring every player gets the exact same "random" track.

Think of the random number generator like a giant cookbook, and the seed is the page number. If everyone in a lobby is told to open to **page 58302**, they will all see the same recipe and get the exact same result.

The numbers aren't truly random—they are **pseudo-random**. This means they appear random, but they are part of a fixed, predictable sequence. The seed guarantees that every player's game follows the exact same sequence of choices when building the track, resulting in an identical map for everyone in the race.
