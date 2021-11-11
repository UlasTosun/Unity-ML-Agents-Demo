# Unity-ML-Agents-Demo

This is a template and demo project for Unity ML-Agents Toolkit. **[You can find complete Unity project folder in here.](https://drive.google.com/drive/folders/1i45Nl_Ew3kjEa1NUfoKRF3wzHv-Z00bp?usp=sharing)** Since, GitHub has file count and size limitations, I have uploaded the project to Google Drive.

Project has been developed on Unity 2021.2.0f1 by using ML Agents Package 2.0.0. If you are facing with errors, **please check your Unity and package versions**. Please keep in my that using different versions may result with errors.

## Installation

There are multiple steps to create a proper setup to use ML-Agents Toolkit. [You can find the detailed installation steps in here.](https://github.com/Unity-Technologies/ml-agents/blob/main/docs/Installation.md)

I have already add ML Agents Package to the project by using package manager. Also, I have already download ML-Agents Repository and copied it to the project folder. So, you can skip these steps. Only thing you should do is installing a Python environment which includes both PyTorch and ML-Agents Python package. Please check the link above for further details. You can also read this [getting started guide](https://github.com/Unity-Technologies/ml-agents/blob/main/docs/Getting-Started.md) for basics of ML-Agents Toolkit.

## Environment

Environment includes a target object, a non-target object and a wall object. The agent should not hit the non-target object while hitting the target. You can simply assume that, the target object is the bad guy and the non-target object is a hostage. Positions and sides of target and non-target changes at the beginning of every episode. One of them is placed on left and the other one is placed on the right side, ramdomly. Also, their positions on their sides change a little bit in X-axis.

In additon, both agent and wall spawn in random positions with random rotations at the beginnig of every episode.

Those randomizations are applied to prevent overfitting. Otherwise, the agent become too environment-specific and fragile against even minor changes in the environment.

You can see the environment below. The wall intentionally desinged half-transparent to be able to see what the agent does behind the wall. However, the agent cannot see what is behind the wall (wall is opaque for the agent).

<img src="/Images/Environment.jpg" width="900" height="400">

## Goal

Agent should hit the target and should not hit the non-target. Agent should not drop down from ground.

## Actions

There are 3 kinds of actions that agent can do:

1) **Fire:** do not fire or fire.
2) **Rotate:** do not rotate, rotate left, rotate right
3) **Move:** do not move, move forward, move forward faster

The agent can choose an action from each kind. So, it can choose firing, rotating left and moving forward at the same time. However, there is an order for actions on implementation. Firstly, it fires, then it rotates and finally it moves forward. For example, if it chooses "firing, rotating left and moving forward", then it fires the bullet through the current orientation, rotates left and move through new direction, respectively.

## Rewards and Penalties

Please note that, negative rewards are used as penalties in reinforcement learning.

1) **Hit on target:** +1
2) **Hit on non-target:** -1
3) **Dropping from platform (going out of area):** -1
4) **Missing the target while firing:** -0.1 (it should be penaltized for misses, otherwise it keeps firing all time)
5) **Both rotating and moving:** -0.001 (to force it to complete the mission in as less steps as possible)

## Observations

The agent observes the environment by using 2 ray perceptions sensors and vector observations.

1) **Short range ray perceptions sensor:** observes ground to prevent droping down from ground.
2) **Long range ray perceptions sensor:** observes wall, target and non-target.
3) **Vector observations:** observes agent's positon, rotation and velocity, target positon, distance to the target, etc.

## Behaviors

There are 3 kinds of behaviors in Unity ML-Agents Toolkit. Please be sure that you are using the correct behavior for your aim.

1) **Default:** for training the agent
2) **Heuristic Only:** for testing both agent and environment
3) **Inference Only:** for using trained models

To change the behavior find "Assets -> Prefabs -> Agent -> Inspector -> Behavior Parameters -> Behavior Type".

## Training

Currently, the agent is trained by using PPO (Proximal Policy Optimization) with extrinsic rewards enabled for more than 2500 episodes (more than 4 hours in my machine). Neural network has 3 hidden layers with 256 hidden units. During training, 8 agents trained at the same time for a faster and more stable training process. Following video was recorded during training.

https://user-images.githubusercontent.com/40580957/141371446-d637404a-44f4-45bb-bf9f-de7cfdfa719c.mp4

At the end of training, cumulative reward is reached to 0.614. Since the environment has too much penalties, 0.614 is an acceptable score for this scenario. Please remember that every missing shot results with a penalty of 0.1. You can find detailed traning statistics below.

<img src="/Images/PPO_TB.jpg">

Please note that, there are 3 trained models are in "Assets -> Trained Models" and they are named as "Shooter-XXX.onnx". The numbers in model names represents how many steps the agent trained for. So, higher number means more trained and better model.

Additionally, you can train your own agent by following [these steps.](https://github.com/Unity-Technologies/ml-agents/blob/main/docs/Training-ML-Agents.md)

## Inference

Following video shows how the agent behaves to complete the mission.

https://user-images.githubusercontent.com/40580957/141372999-bb1f23b8-1dd5-426d-8349-57c10f5d693f.mp4

## Test

You can test both environment and the agent in heuristic behavior. You can control the agent by using following keys:

1) "Space" -> fire
2) "S" -> move forward, "W" -> move forward faster
3) "A" -> rotate left, "D" -> rotate right

## Roadmap

1) More models which are trained by using different methods such as SAC (Soft Actor-Critic) and BC (Behavioral Clonning) will be added.
2) Comparison of different models will be added.
3) An Android build for inference will be provided.
