# Representation-of-a-Real-World-Problem-as-a-Markov-Decision-Process

## Aim

To model an autonomous lunar rover exploring and mapping the Moon's surface as a Markov Decision Process (MDP) by defining its states, actions, transition probabilities, rewards, and implementing the model using Python dictionaries.

## Problem Statement

An autonomous lunar rover is deployed on the Moon to explore and map unknown terrain. The rover must navigate the lunar surface, scan unexplored areas, and return safely to its base.

The problem is represented as an MDP to model sequential decision-making under uncertainty. The objective is to maximize the reward obtained from exploring and mapping the Moon's surface.

### Problem Description

A lunar rover is designed to explore and map the Moon's surface autonomously. It moves to unexplored terrain, scans the region to collect mapping data, and returns safely after completing its exploration. The MDP represents the rover's possible states, available actions, transition probabilities, and rewards.

## MDP Components

A Markov Decision Process is represented as:

$$
MDP = (S, A, P, R, \gamma)
$$

Where:

| Symbol | Meaning |
|---|---|
| \(S\) | Set of states |
| \(A\) | Set of actions |
| \(P\) | Transition probability function |
| \(R\) | Reward function |
| \(\gamma\) | Discount factor |

---

## State Space

The state space represents the possible situations of the lunar rover.

```text
S = {
    S0: Rover at base,
    S1: Exploring unexplored terrain,
    S2: Mission completed
}
```

### State Descriptions

| State | Description |
|---|---|
| S0 | The rover is at its base, ready to explore. |
| S1 | The rover is exploring unexplored terrain. |
| S2 | The rover has completed its mission and returned safely. |

---

## Sample State

A sample state is:

```text
S1 = Exploring unexplored terrain
```

In this state, the rover can scan the terrain to collect mapping data or return to its base.

```python
sample_state = "S1"
print("Sample State:", sample_state)
```

---

## Action Space

The action space defines the actions available to the rover.

```text
A = {
    A0: Move,
    A1: Scan,
    A2: Return
}
```

### Action Descriptions

| Action | Description |
|---|---|
| A0 | Move to unexplored terrain. |
| A1 | Scan the terrain and collect mapping data. |
| A2 | Return safely to base. |

---

## Sample Action

A sample action is:

```text
A1 = Scan
```

The rover scans the current terrain to collect mapping data.

```python
current_state = "S1"
action = "Scan"

print("Current State:", current_state)
print("Selected Action:", action)
```

---

## Transition Probability

The transition probability describes the probability of moving from one state to another after an action is performed.

The general form is:

$$
P(s' \mid s,a)
$$

Where:

- \(s\) is the current state.
- \(a\) is the selected action.
- \(s'\) is the next state.

### Transition Table

| Current State | Action | Next State | Probability |
|---|---|---|---:|
| S0 | Move | S1 | 1.0 |
| S1 | Scan | S1 | 0.2 |
| S1 | Scan | S2 | 0.8 |
| S1 | Return | S2 | 1.0 |

The probabilities for each state-action pair sum to 1.

For example:

$$
P(S2 \mid S1,\text{Scan}) = 0.8
$$

This means that scanning unexplored terrain has an assumed 80% probability of transitioning to the mission-completed state in this simplified model.

---

## Reward Function

The reward function defines the feedback received by the rover after taking an action.

The general form is:

$$
R(s,a,s')
$$

### Reward Table

| Current State | Action | Next State | Reward |
|---|---|---|---:|
| S0 | Move | S1 | +5 |
| S1 | Scan | S1 | -1 |
| S1 | Scan | S2 | +10 |
| S1 | Return | S2 | +5 |

Positive rewards encourage exploration and mission completion. A negative reward represents the cost of an unsuccessful scan.

The objective is to maximize the cumulative discounted reward:

$$
G_t = \sum_{k=0}^{\infty}\gamma^k R_{t+k+1}
$$

The discount factor is:

$$
\gamma = 0.9
$$

---

## Graphical Representation

```mermaid
flowchart TD
    S0["S0: Rover at Base"]
    S1["S1: Exploring Terrain"]
    S2["S2: Mission Completed"]

    S0 -->|"Move, P=1.0, R=+5"| S1
    S1 -->|"Scan, P=0.2, R=-1"| S1
    S1 -->|"Scan, P=0.8, R=+10"| S2
    S1 -->|"Return, P=1.0, R=+5"| S2
```

---

## Python Representation

The following Python program represents the MDP using dictionaries. Each transition uses the tuple format:

`(probability, next_state, reward, terminated)`

```python
# Representation of a Real-World Problem as an MDP
# Problem: Lunar Rover Exploration

print("Name: ROHITH S")
print("Register Number: 212225240121")
print()

# 1. State Space
states = {
    0: "Rover at Base",
    1: "Exploring Unexplored Terrain",
    2: "Mission Completed"
}

# 2. Action Space
actions = {
    0: "Move",
    1: "Scan",
    2: "Return"
}

# 3. Transition Probability, Reward, and Termination
# Format: (probability, next_state, reward, terminated)

P = {
    0: {
        0: [(1.0, 1, 5, False)],
        1: [(1.0, 1, 0, False)],
        2: [(1.0, 2, 5, True)]
    },

    1: {
        0: [
            (0.8, 1, 5, False),
            (0.2, 2, 10, True)
        ],
        1: [
            (0.2, 1, -1, False),
            (0.8, 2, 10, True)
        ],
        2: [
            (1.0, 2, 5, True)
        ]
    },

    2: {
        0: [(1.0, 2, 0, True)],
        1: [(1.0, 2, 0, True)],
        2: [(1.0, 2, 0, True)]
    }
}

# 4. Discount Factor
gamma = 0.9

# 5. MDP Representation
mdp = {
    "states": states,
    "actions": actions,
    "P": P,
    "gamma": gamma
}

# 6. Display MDP Dictionary
print("=== MDP DICTIONARY ===")
print("P =", P)

# 7. Display All Transition Details
print("\n=== TRANSITION DETAILS ===")

for state, action_data in P.items():
    for action, outcomes in action_data.items():
        for probability, next_state, reward, terminated in outcomes:
            print("Current State:", state, "-", states[state])
            print("Action:", action, "-", actions[action])
            print("Next State:", next_state, "-", states[next_state])
            print("Probability:", probability)
            print("Reward:", reward)
            print("Terminated:", terminated)
            print("-" * 40)

# 8. Sample State and Action
current_state = 1
selected_action = 1

print("\n=== SAMPLE TRANSITION ===")
print("Current State:", current_state, "-", states[current_state])
print("Selected Action:", selected_action, "-", actions[selected_action])
print("Possible Outcomes:", P[current_state][selected_action])

print("\nDiscount Factor:", gamma)
print("MDP representation created successfully.")
```

**Note:** The dictionary includes all three actions for every state to match the structure of the supplied example. State `2` is terminal; its self-transitions are included only for structural consistency and are not intended to represent actions taken after the mission ends.

---

## Output

The program prints the full dictionary in tuple format and then displays each transition's current state, action, next state, probability, reward, and termination status.

```text
Name: ROHITH S
Register Number: 212225240121

=== MDP DICTIONARY ===
P = {0: {0: [(1.0, 1, 5, False)], 1: [(1.0, 1, 0, False)], 2: [(1.0, 2, 5, True)]}, 1: {0: [(0.8, 1, 5, False), (0.2, 2, 10, True)], 1: [(0.2, 1, -1, False), (0.8, 2, 10, True)], 2: [(1.0, 2, 5, True)]}, 2: {0: [(1.0, 2, 0, True)], 1: [(1.0, 2, 0, True)], 2: [(1.0, 2, 0, True)]}}

=== TRANSITION DETAILS ===
Current State: 0 - Rover at Base
Action: 0 - Move
Next State: 1 - Exploring Unexplored Terrain
Probability: 1.0
Reward: 5
Terminated: False
----------------------------------------

Current State: 1 - Exploring Unexplored Terrain
Action: 0 - Move
Next State: 1 - Exploring Unexplored Terrain
Probability: 0.8
Reward: 5
Terminated: False
----------------------------------------

Current State: 1 - Exploring Unexplored Terrain
Action: 0 - Move
Next State: 2 - Mission Completed
Probability: 0.2
Reward: 10
Terminated: True
----------------------------------------

Current State: 1 - Exploring Unexplored Terrain
Action: 1 - Scan
Next State: 1 - Exploring Unexplored Terrain
Probability: 0.2
Reward: -1
Terminated: False
----------------------------------------

Current State: 1 - Exploring Unexplored Terrain
Action: 1 - Scan
Next State: 2 - Mission Completed
Probability: 0.8
Reward: 10
Terminated: True
----------------------------------------

Discount Factor: 0.9
MDP representation created successfully.
```

*The output above shows representative transitions. Running the code prints all transitions, including those for the `Return` action and the terminal state.*

---

## Result

The lunar rover exploration problem was successfully represented as a Markov Decision Process using Python dictionaries. The model contains three states and three actions. Each transition specifies the probability, next state, reward, and termination status in a tuple. The program displays these values explicitly, demonstrating how the rover can explore terrain, collect mapping data, and complete its mission.
