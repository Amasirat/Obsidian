
One of the newest fields of engineering and science. The term was first coined in 1956.

**An agent** is anything that acts inside a system. It's basically a generic term for an AI entity whether it be a software or hardware+software one.

There have been discussion as to whether its better to model **humans** for developing AIs or the process of being **rational**.
## Being Human

The turing test was designed to test how human an agent is. The test includes a judge that would ask questions of an unknown entity that it does not know. If it can correctly guess if this entity is an AI 30% of the time, then the AI has failed the test.

This test can not really help us in actually creating intelligent agents, because mimicking a human speech and fooling another human is not a correct objective of intelligence since masterful imitation could get us reasonably there. However it is important because despite all of that, passing this test will require solving quite a lot of complex problems.

* Natural Language Processing: Ability to parse and create correct sentences.
* Storing what it knows or hears
* Machine Learning
* automated reasoning, to reason about and answer questions given what it knows and draw new conclusions

In general, trying to emulate a human will not give us the best results, since humans are **flawed**.
That is why another suggested method of modeling AI is being developed, and that is what we study here.

A summary:

* **Human Thinking**: We don't know how our thoughts work, so the task is too complex
* **Human Behavior**: The task will not give us the best result since human behavior is **flawed**
* **Rational Thinking**: The laws of Boolean algebra first codified by Aristotle, however computations can be incredibly intensive
* **Rational Behavior**: It includes rational thinking and is concerned with doing the **right** thing in every given situation.
**Rational Behavior** is what we'll be striving for in this study.
# Chapter 2: Intelligent Agents

Agents are beings that **percieve** and **Take Action** inside their environment. An agent can be software or a combination of software and hardware.

For example, take a vacuum cleaning agent:

* There are two or more rooms, each one can either be clean or dirty.
* The agent has four choices: **Go Left**, **Go Right**, **Suck**, **Do Nothing**
* Its perceptions are limited to the room it is currently in.
* It can not affect any location that it is not in.

But is this agent **rational**? How do we measure such a thing?

There are four factors that determine an agent's rationality:

 * Its **Performance Measure**
 * Its prior knowledge of the **Environment**
 * Its **Sensors** and its perception history
 * The **Actions** it takes given its perception history.

These factors are shortened to **PEAS**:
* **Performance**
* **Environment**
* **Actuators**
* **Sensors**
Given this, our definition of a **rational agent** will be:

***An agent that given its perception history, the set of actions it can take, and also its environment, performs a set of actions that maximize its performance measure***

An agent that is **autonomous**, tries to **Gather Iniformation** on its own through its series of perceptions and takes actions rather than the programmer feeding it with information. **That's what seperates AI from other types of computer programs**

It is not required for an agent to be **All-knowing** to be rational, it must simply choose a **correct** choice given the information it currently has.
## Performance Measure

Designers of an agent, have to define a correct performance measure or an objective that an agent has to reach. **If a performance measure is not chosen correctly, the agent can be incentivized to perform actions that are not desirable.**

For example, if our performance measure of the vacuum cleaner agent were the amount of dirt it picked up, it would incentivize the agent to clean a floor, then push back all of that dirt and clean it again so that this performance measure is maximized. However if we choose the performance measure based on the cleanliness of the environment in the most time steps, then the agent will perform as expected.

*Generally, performance measure should usually involve the change we want in the environment as opposed to the agent itself.*
## Environment

Different types of environments:
* **Fully Observable** vs **Partially Observable**: If an agent has access to all the information it requires from its environment as opposed to **Partially Observable** where some information is hidden from it.

* **Single-Agent** vs **Multi-Agent**: An environemnt where an agent is alone as opposed to a **Multi-Agent** environment where an agent is either in **competition** or **cooperation** with another agent.

* **Deterministic** vs **Stochastic**: In an environment where each action has a determined consequence that can be predicted, as opposed to a **Stochastic** environment where a decision might have a **probability** of several consequences.

* **Episodic** vs **Sequential**: Decisions of the agent can be seperated into atomized units without the affect of its prior decisions as opposed to **sequential** where each decision leads to the next.

* **Static** vs **Dynamic**: While the agent deliberates (Thinks and tries to decide on an action), the environment does not change as opposed to **Dynamic** Environments. If however the change is something related to the agent itself, for instance a timer, that environment would be **Semi-Dynamic**.

* **Known** vs **Unknown**: An environment where the agent knows the rules and knows what would happen by its actions as opposed to **Unknown**. It is different from **deterministic** or **Stochastic** environments in that, even in stochastic environments, each action has a **probability** of a consequence but in **Unknown** environments, the action is *trully* unknown.

# Structure of Agents

There are four types of agent programs:

* **Simple-Reflex Agents**: Simplest kind of agent, it takes action based on its **Current perception** of its environment using a series of condition rules.
	* Simple but not intelligent
	* Can only do well in **Fully Observable Environments**
	* If its goal changes, it'll be useless (because its conditions and actions are hardwired and it can not learn new rules and environments)
* **Model-Based Reflex Agents**: Its a reflex agent with the difference that it **saves a summary of its experience** I.E **State**.
	* Can be used in **Partially-Observable** Environments
	* Records the state that it is in
* **Goal-Based Agents**: They are model-based agents that do actions to service a particular goal
* **Utitlity-Based Agents**: It focuses on making decisions that maximize a **Utility Function** that is derived from the **Performance Measure**
		For example, an agent that has the goal of reaching a destination, could have a utility function that makes it want to maximize its speed so that it can get there faster. If this **utility function** is simillar to the **Expected Performance Measure** then the agent is being **rational** and is working towards its own **happiness**.(Utility)

There's also a new type that has been worked on recently which is called **Learning Agents**.

A **Learning Agent** Contains:
* **Critic**: gives feedback to the agent
* **Perception Element**: The part of the agent that acts and percepts
* **Learning Element**: Takes in feedback from the critic and knowledge from Perception Element and results in it changing the performance element as well.
* **Problem Generator**: Encourages the agent to **explore** and gives it new problems to solve (Basically brings the agent out of its comfort zone)

![[2024-10-26_18-32.png]]

# Chapter 3: Searching Algorithms

