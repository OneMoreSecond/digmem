# Philosophy of this project

## Towards good memory

Memory is a representation of history equipped with a retrieval index.

### History

History is what happened.

However, nobody can capture every detail of a moment. Only a representation can remain.

### Good memory

Memory is created for future, with two metrics:

- Gain: memory should make future tasks easier
- Cost: memory lookup should be affordable

From this view, memory is a temporal information bottleneck.

#### Gain landscape in a task

| Retrieval result | Gain in this task |
| --- | --- |
| Related and correct | positive |
| Unrelated | zero |
| Related and incorrect | negative |

#### Type of cost for agent

- Token cost
- Attention cost: retrieved information may distract the agent.

### Challenges

#### Predicting

Memory should be optimized for future.
But what's the future like?

#### Representation & Retrieval

What should remain in history representation?

How to access the representation effectively?

#### Evolvement

How to incorporate latest information?

## Method: single-rollout RL

### Reward from LLM verifier

It can be hard for LLM agent to:

- find a solution for certain task (which may require exhaustive search)
- pick the solution with long-term reward in the mind ("taste")

But it's much easier to evaluate instant reward of a finished task.
If any unhappiness is in the trajectory, that's a signal.

Because the task is finished finally, the trajectory also provides enough information for verifier to assign the credit for unhappiness.

### History as training data

It's hard to estimate future distribution, but history is a sample.
The history will grow and get closer to the distribution.

Full history must be saved as the data set.
After a new task is finished, the index can be updated based on the signal.
