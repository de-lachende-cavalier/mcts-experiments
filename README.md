# Monte Carlo Tree Search experiments

This repo provides a simple and clean implementation of MCTS for perfect information domains. The main work is due to [kstruempf](https://github.com/kstruempf), whose fork served as my starting point (because I liked the additions he made) and whom I necessarily must thank!

The code within this repo is mostly didactic and useful for my experiments, whatever they may be. If you want to use it, clone the repo and run things locally.

If you want to get the library (by kstruempf), then check the installation section [here](https://github.com/kstruempf/MCTS).

## Quick Usage

In order to run MCTS, you must implement your own `State` class that extends `mcts.base.base.BaseState` which can fully
describe the state of the world. It must implement four methods:

- `get_current_player()`: Returns 1 if it is the maximizer player's turn to choose an action, or -1 for the minimiser
  player
- `get_possible_actions()`: Returns an iterable of all `action`s which can be taken from this state
- `take_action(action)`: Returns the state which results from taking action `action`
- `is_terminal()`: Returns `True` if this state is a terminal state
- `get_reward()`: Returns the reward for this state. Only needed for terminal states.

You must also choose a hashable representation for an action as used in `get_possible_actions` and `take_action`.
Typically, this would be a class with a custom `__hash__` method, but it could also simply be a tuple, a string, etc.
A `BaseAction` class is provided for this purpose.

Once these have been implemented, running MCTS is as simple as initializing your starting state, then running:

```python
from mcts.base.base import BaseState
from mcts.searcher.mcts import MCTS


class MyState(BaseState):
    def get_possible_actions(self) -> [any]:
        pass

    def take_action(self, action: any) -> 'BaseState':
        pass

    def is_terminal(self) -> bool:
        pass

    def get_reward(self) -> float:
        pass

    def get_current_player(self) -> int:
        pass


initial_state = MyState()

searcher = MCTS(time_limit=1000)
bestAction = searcher.search(initial_state=initial_state)
```

Here the unit of `time_limit=1000` is milliseconds. You can also use for example `iteration_limit=100` to specify the
number of rollouts. Exactly one of `time_limit` and `iteration_limit` should be specified.

```python
best_action = searcher.search(initial_state=initial_state)
print(best_action)  # the best action to take found within the time limit
```

To also receive the best reward as a return value set `need_details` to `True` in `searcher.search(...)`.

```python
best_action, reward = searcher.search(initial_state=initial_state, need_details=True)
print(best_action)  # the best action to take found within the time limit
print(reward)  # the expected reward for the best action
```

## Examples

You can find some examples using the MCTS here:

- [naughtsandcrosses.py](https://github.com/kstruempf/MCTS/blob/main/mcts/example/naughtsandcrosses.py) is a minimal
  runnable example by [pbsinclair42](https://github.com/pbsinclair42)
- [connectmnk.py](https://github.com/kstruempf/MCTS/blob/main/mcts/example/connectmnk.py) is an example running a full
  game between two MCTS agents by [LucasBorboleta](https://github.com/LucasBorboleta)

## Collaborating

Feel free to raise a new issue for any new feature or bug you've spotted. Pull requests are also welcomed if you're
interested in directly improving the project.
