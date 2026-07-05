# tabular-rl

Reinforcement Learning algorithms with nothing abstracted away: MDPs, value functions, Q-functions,
and policies are all explicit objects, so you can read exactly how each algorithm manipulates them.

## Algorithms

| Algorithm | Entry point |
| --- | --- |
| Value Iteration | `ValueFunctionHelpers.value_iteration` in `src/dynamic_programming/valuefunctionhelpers.py` |
| Policy Iteration (Sutton & Barto style: iterative policy evaluation, then greedy improvement) | `PolicyHelpers.policy_iteration_sutton` in `src/dynamic_programming/policyhelpers.py` |
| Policy Iteration (Brunskill style: one Q-function backup per improvement step) | `PolicyHelpers.policy_iteration_brunskill` in `src/dynamic_programming/policyhelpers.py` |
| Iterative Policy Evaluation | `PolicyHelpers.evaluate_policy` in `src/dynamic_programming/policyhelpers.py` |
| Monte Carlo Prediction (first-visit and every-visit) | `MonteCarloHelpers.monte_carlo_policy_evaluation_{first,every}_visit` in `src/monte_carlo/montecarlohelpers.py` |
| Monte Carlo Control (first-visit and every-visit, epsilon-greedy) | `MonteCarloHelpers.monte_carlo_control_{first,every}_visit` in `src/monte_carlo/montecarlohelpers.py` |

## Layout

- `src/building_blocks/` — the core objects: `State`, `ProbabilisticAction`, `MarkovDecisionProcess`,
  `Policy`, `ValueFunction`, `QFunction`.
- `src/dynamic_programming/` — planning algorithms (value iteration, policy iteration).
- `src/monte_carlo/` — model-free prediction and control.
- `src/factories/` — convenience factories for building states and actions.
- `src/problems/gridworld.py` — the classic 3x4 grid world used by the tests, with stochastic movement
  (0.8 intended direction, 0.1 each perpendicular), a +100 terminal state and a -100 terminal state.

## Getting started

```bash
pip install -r requirements.txt
```

Solve the grid world with value iteration:

```python
from src.problems.gridworld import GridWorld
from src.dynamic_programming.valuefunctionhelpers import ValueFunctionHelpers

mdp = GridWorld.get_game()
policy, value_function = ValueFunctionHelpers.value_iteration(mdp, discount_factor=0.9)
print(policy)
```

Or with policy iteration:

```python
from src.problems.gridworld import GridWorld
from src.dynamic_programming.policyhelpers import PolicyHelpers

mdp = GridWorld.get_game()
policy, mdp = PolicyHelpers.policy_iteration_sutton(mdp, discount_factor=0.9)
```

Or model-free, with Monte Carlo control:

```python
from src.problems.gridworld import GridWorld
from src.monte_carlo.montecarlohelpers import MonteCarloHelpers

mdp = GridWorld.get_game()
policy = MonteCarloHelpers.monte_carlo_control_first_visit(
    mdp, discount_factor=0.9, stable_count=3, exploration_ratio=0.2, episodes_count=100)
```

Run the tests (from the repository root):

```bash
python -m pytest
```

## Resources

What is the potential of RL?
  * [Playing Atari with Deep Reinforcement Learning](https://deepmind.com/research/publications/playing-atari-deep-reinforcement-learning)
  * [AlphaZero: Shedding new light on chess, shogi, and Go](https://deepmind.com/blog/article/alphazero-shedding-new-light-grand-games-chess-shogi-and-go)
  * [DeepMind: The podecast](https://www.youtube.com/playlist?list=PLqYmG7hTraZBiUr6_Qf8YTS2Oqy3OGZEj)

Lectures?
  * [Stanford's Emma Brunskill](https://www.youtube.com/playlist?list=PLoROMvodv4rOSOPzutgyCTapiGlY2Nd8u)
  * [UCL's David Silver](https://www.youtube.com/playlist?list=PLqYmG7hTraZDM-OYHWgPebj2MfCFzFObQ)

Reference Book?
  * [Sutton-Barto](http://incompleteideas.net/book/bookdraft2017nov5.pdf)

### Planning Resources
   * [Grid World Video](https://youtu.be/bHeeaXgqVig)
   * [Value Iteration, Poole & Mackworth, UBC](https://artint.info/html/ArtInt_227.html)
   * [MDPs, Value Iteration, Policy Iteration, and Linear Programming, Abbeel, Berkley](https://people.eecs.berkeley.edu/~pabbeel/cs287-fa12/slides/mdps-exact-methods.pdf)
   * [MDPs, Farhadi, UW](https://courses.cs.washington.edu/courses/csep573/14sp/slides/6_MDP.pdf)
