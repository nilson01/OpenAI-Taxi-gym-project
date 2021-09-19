# OpenAI Gym's Taxi project

### Getting Started

The description of the environment is in subsection 3.1 of [this paper](https://arxiv.org/pdf/cs/9905014.pdf).  You can verify that the description in the paper matches the OpenAI Gym environment by peeking at the code [here](https://github.com/openai/gym/blob/master/gym/envs/toy_text/taxi.py).

### MAP

    "+---------+",
    "|R: | : :G|",
    "| : | : : |",
    "| : : : : |",
    "| | : | : |",
    "|Y| : |B: |",
    "+---------+",



    The Taxi Problem
    #########################
    #########################
    
    From "Hierarchical Reinforcement Learning with the MAXQ Value Function Decomposition"
    by Tom Dietterich
    
    
    Description:
    
    There are four designated locations in the grid world indicated by R(ed), G(reen), Y(ellow), and B(lue). When the episode starts, the taxi starts off at a random square and the passenger is at a random location. The taxi drives to the passenger's location, picks up the passenger, drives to the passenger's destination (another one of the four specified locations), and then drops off the passenger. Once the passenger is dropped off, the episode ends.
    
    
    Observations:
    
    
    There are 500 discrete states since there are 25 taxi positions, 5 possible locations of the passenger (including the case when the passenger is in the taxi), and 4 destination locations.
    Note that there are 400 states that can actually be reached during an episode. The missing states correspond to situations in which the passenger is at the same location as their destination, as this typically signals the end of an episode.
    
    
    Four additional states can be observed right after a successful episodes, when both the passenger and the taxi are at the destination.
    
    
    This gives a total of 404 reachable discrete states.
    Passenger locations:
    - 0: R(ed)
    - 1: G(reen)
    - 2: Y(ellow)
    - 3: B(lue)
    - 4: in taxi
    
    
    Destinations:
    - 0: R(ed)
    - 1: G(reen)
    - 2: Y(ellow)
    - 3: B(lue)
    
    
    Actions:
    There are 6 discrete deterministic actions:
    - 0: move south
    - 1: move north
    - 2: move east
    - 3: move west
    - 4: pickup passenger
    - 5: drop off passenger
    
    
    Rewards:
    There is a default per-step reward of -1,
    except for delivering the passenger, which is +20,
    or executing "pickup" and "drop-off" actions illegally, which is -10.
    
    
    Rendering:
    - blue: passenger
    - magenta: destination
    - yellow: empty taxi
    - green: full taxi
    - other letters (R, G, Y and B): locations for passengers and destinations
    state space is represented by:
        (taxi_row, taxi_col, passenger_location, destination)
    """




### Instructions

The repository contains three files:
- `agent.py`: We develop our reinforcement learning agent here.  
- `monitor.py`: The `interact` function tests how well an agent learns from interaction with the environment.
- `main.py`: Run this file in the terminal to check the performance of your agent.

You can begin by running the following command in the terminal:
```
python main.py
```

When you run `main.py`, the agent that you specify in `agent.py` interacts with the environment for 20,000 episodes.  The details of the interaction are specified in `monitor.py`, which returns two variables: `avg_rewards` and `best_avg_reward`.
- `avg_rewards` is a deque where `avg_rewards[i]` is the average (undiscounted) return collected by the agent from episodes `i+1` to episode `i+100`, inclusive.  So, for instance, `avg_rewards[0]` is the average return collected by the agent over the first 100 episodes.
- `best_avg_reward` is the largest entry in `avg_rewards`.  This is the final score that you should use when determining how well your agent performed in the task.

- With the `__init__()` method to define any needed instance variables, we define the number of actions available to the agent (`nA`) and initialize the action values (`Q`) to an empty dictionary of arrays. Using epsilon we check if the agent uses an epsilon-greedy policy for selecting actions.

- The `select_action()` method accepts the environment state as input and returns the agent's choice of action.  

- The `step()` method accepts a (`state`, `action`, `reward`, `next_state`) tuple as input, along with the `done` variable, which is `True` if the episode has ended. Instead of increments in the action value of the previous state-action pair by 1, we change it  to use the sampled tuple of experience to update the agent's knowledge of the problem.

OpenAI Gym [defines "solving"](https://gym.openai.com/envs/Taxi-v1/) this task as getting average return of 9.7 over 100 consecutive trials which is what we eventually observe at the end of this project.
