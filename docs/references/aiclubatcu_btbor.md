---
id: aiclubatcu_btbor
title: Breaking the Barrier of Robotics
slug: /references/aiclubatcu_btbor
---

#### Thank you all for attending. I thought I would publish a few references to things that came up during our discussion.
  
<img src="/resources/img/20261013_aicu_btbor.jfif" alt="October 13th 2026" width="33%" style={{marginBottom: '24px'}} /> 
  
*NOTE: If there's anything I missed or more that you are curious about please don't hesitate to [email me](mailto:daniel@brainwavecollective.ai). I am happy to update this page and answer any specific questions.*

## Meet Barbara

What a monster we created. Although <a href="/resources/assets/2026.10.13_barbara_initial.md" target="_blank">our initial prompt</a> likely would have been plenty to compel the personality, the results of our refinements can be seen in <a href="/resources/assets/2026.10.13_barbara_final.md" target="_blank">Barbara's Final Prompt</a>). You can reproduce the interactions by copying the prompt to Hugging Face's [realtime voice AI pipeline](https://huggingface.co/spaces/smolagents/hf-realtime-voice) by going to ⚙ Settings and adding the prompt as **instructions** with the voice "Ono_Anna."

## Simulation Environments

During the talk I showed a video about [RL and sim-to-real for the Microduck using MuJoCo](https://www.youtube.com/watch?v=N3evPHMl0aI).

A similar MuJoCo environment exists for the Reachy Mini. You can find more info in the [Reachy Mini simulation guide](https://huggingface.co/docs/reachy_mini/en/platforms/simulation/get_started).

Given LeRobot's broader focus there are many more options available, including: LIBERO, LeIsaac (NVIDIA Isaac Lab), Meta-World, and Gym-Style Tasks. There are also 3rd party integrations with WASM MuJoCo Web Demos, LeHome, and gym-so100-c. You can find more info in the [LeRobot GitHub](https://github.com/huggingface/lerobot).

## Do humans need to do the work? 

In regards to what it takes to bring together the various subsystems I mentioned that Google's new Gemini 4 Argon now has a [robot that trained itself in simulation](https://www.youtube.com/watch?v=9PICEmtfpWU) (also MuJoco) so we expect the future will be one that doesn't even require us to do the work of staging the training. I also showed a video comparing the progress of Boston Dynamics over the last 7 years. Here is a video showing the [acrobatic capabilities of Atlas](https://www.youtube.com/watch?v=UNorxwlZlFk).

I mentioned [VLA planning](https://roboticsfyi.substack.com/p/vision-language-action-explained) as a current approach that makes it easy to orchestrate actions and if you want to go deeper you can explore this [study on heirarchical agents](https://jiahenghu.github.io/hi-vla/) with [related Hi-VLA paper](https://arxiv.org/abs/2606.10267).

## On Risk and Abuse

The quote that I mentioned came from [On War, by Carl von Clausewitz](https://www.gutenberg.org/files/1946/1946-h/1946-h.htm) (1780–1831) where he says “War therefore is an act of violence intended to compel our opponent to fulfil our will." and that "Violence arms itself with the inventions of Art and Science...”.

The application of this to modern computing (and robotics) is referenced by [Grady Booch in his talk about the co-evolution of computing and conflict](https://www.youtube.com/watch?v=Y5zHBXafQnY) where he goes much deeper into these ideas.

## Purchasing Robots 

Although you can [purchase Reachy Mini directly from Hugging Face/Pollen Robotics](https://pollen-robotics.com/reachy-mini/), I have a handful available that I would be willing to sell. I have both Wirless and Lite versions. Feel free to [email me](mailto:daniel@brainwavecollective.ai) if you are interested. Cost will be $50 above what HF charges, but they are already assembled, have Reachy Eyes installed, and you won't have to wait for Hugging Face to ship.

I also have 3D printed kits, motors, and other parts for the SO-101 LeRobot arms.

Microduck is currently pre-order and is expected to ship early next year.






