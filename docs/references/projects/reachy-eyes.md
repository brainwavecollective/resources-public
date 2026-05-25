---
id: reachy-eyes
title: Reachy Eyes
sidebar_label: Reachy Eyes
slug: /references/projects/reachy-eyes
---

**Reachy Eyes** is the first third-party hardware product for the [Reachy Mini](https://www.pollen-robotics.com/reachy-mini/), created by community member [Daniel Ritchie](https://github.com/brainwavecollective) of the Brain Wave Collective. It adds programmable, expressive LED eyes to the robot, extending its emotional range and enabling new forms of color-based communication.

> 📦 **GitHub:** [brainwavecollective/reachy-eyes](https://github.com/brainwavecollective/reachy-eyes)  
> 🤗 **Model:** [danielritchie/vibe-color-model](https://huggingface.co/danielritchie/vibe-color-model)  
> 🤗 **Dataset:** [danielritchie/cinematic-mood-palette](https://huggingface.co/datasets/danielritchie/cinematic-mood-palette)

---

## What It Is

For a social robot, eyes are one of the most expressive channels available. Reachy Eyes gives the Reachy Mini a way to communicate emotion, mood, and state through color, extending the robot beyond gesture and speech into a visual, ambient dimension.

The product includes physical LED eye hardware, a Python control library, onboard firmware, and an embedded ML model that maps emotional context to eye color automatically.

---

## Key Capabilities

**Color & Light** — Support for named colors plus full RGB control. Colors can be set per-eye or both simultaneously, with instant snaps or smooth crossfades. Dynamic parameters such as intensity, energy pulsing, and asymmetry, give the eyes an organic living quality.

**Expressive Effects** — One-shot effects include blink/wink, acknowledgment pulse, and startle spike. A background behavior engine fires natural randomized blinks (including occasional double-blinks) to keep the robot feeling present without active management.

**ML-Driven Emotion (ANIMA)** — The onboard ML model accepts a five-dimensional emotional state vector (valence, arousal, dominance, complexity, coherence) and maps it to eye color. Joyful states produce warm, energetic colors; melancholy reads as cool and muted. Designed to connect directly to LLM output or sentiment pipelines. The companion [Anima affect engine](https://github.com/brainwavecollective/anima-engine) derives these emotional coordinates from text or conversation.

**Character Animations** — Built-in animations include **weewoo** (alternating red/blue, American siren style) and **pinpon** (rapid triple-pulse, French siren style).

---

## Integration

The eyes connect via USB and are auto-discovered. The Python package integrates with ROS2, reachy-sdk, or any custom pipeline. Operates headlessly once configured.

The onboard ML model was trained on a [curated dataset](https://huggingface.co/datasets/danielritchie/cinematic-mood-palette) of color palettes derived from cinematic sources — capturing film's rich vocabulary of color-as-emotion. You can also bypass the model entirely and drive the eyes with any external logic via serial commands.

Full API documentation and DIY build instructions are in the [GitHub repository](https://github.com/brainwavecollective/reachy-eyes).

---

*Reachy Eyes is an independent community project and is not affiliated with or officially endorsed by Pollen Robotics.*
