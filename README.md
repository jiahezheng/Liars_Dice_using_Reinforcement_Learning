# RL_on_Liars_Dice

## Introduction
This project develops an artificial intelligence agent that utilizes Reinforcement Learning methodologies for proficient gameplay in Liar's Dice. The agent is designed to understand and strategically navigate the complexities of the game, making calculated decisions based on the gameplay dynamics.

## Game Rules

### Objective
The aim is to be the last player with dice remaining.

### Setup
Each player (m) starts with n standard six-sided dice and a cup.

### Gameplay

#### Rolling Dice
At the start of each round, players simultaneously roll their dice, keeping the results hidden under their cups.

#### Making Claims
The first player makes a claim about what they think has been collectively rolled by all players. Claims include both a quantity and a face value of the dice.

#### Increasing Claims
Going clockwise, each player must then either make a higher claim or challenge the previous player's claim. A higher claim can be an increase in the quantity of dice, the face value, or both.

#### Challenging (Dudo)
If a player doubts the current claim, they can challenge by declaring "Dudo" (meaning ''I doubt it'' in Spanish). This prompts all players to reveal their dice.

#### Resolving a Challenge
- If the actual total of the claimed dice is **greater than** the claim, the challenger loses a die. (This occurs when the claimer is telling the conservative truth).
- If the total is **less than or equal to** the claim, the player who made the claim loses a die. (This occurs when the claimer is bluffing).
- If the total is **exactly equal** to the claim, the challenger loses a die. (This occurs when the claimer is telling the conservative truth).

#### Losing Dice
Lost dice are placed in full view of all players. Players continue with fewer dice in subsequent rounds.

### Ending the Game
The game continues until only one player has dice remaining, declaring them the winner.

### Example Round

Ann, Bob, and Cal are playing. Bob claims "seven 6s". Cal challenges Bob's claim.

Possible outcomes:
- If there are more than seven 6s, Cal loses a die.
- If there are fewer than seven 6s, Bob loses a die.
- If there are exactly seven 6s, Cal loses a die.

Keep in mind, the strategy lies in making believable claims and knowing when to challenge your opponents!

## Algorithm
This repository contains the implementation of a Reinforcement Learning (RL) agent that has been trained to play Liar's Dice with a high level of skill. The agent, referred to as "the Agent", is trained to master the art of making believable claims and knowing when to challenge opponents, which are critical strategies for success in Liar's Dice.

![](https://github.com/jiahezheng/RL_on_Liars_Dice/blob/main/Algorithm_structure.pdf)

> Algorithm Structure

### Agent's Strategy

The Agent's strategy is defined by a Q-table which takes into account its current dice roll and the last claim made by an opponent. The Q-table is structured as follows:

`Q(Agent's Rolls, Last Claim) = [P_doubt, P_claim_1, P_claim_2, ..., P_claim_n]`

Here, each claim `P_claim_x` represents a possible claim that is stronger than the last claim made. Due to the vast number of possible dice combinations, the Q-table can become quite large, leading to significant computational costs. For demonstration purposes, our implementation showcases the algorithm for 3 players, each with 3 dice.

For example:
- The agent rolls one 1, one 3, and one 6. This is represented in the Q-table as '101001'.
- The last claim made by the previous player is three 4's, represented as '000300'.
- The Agent must now decide whether to doubt this claim or make a higher claim using the Q-table:

`P_doubt, P_claim_1, P_claim_2, ..., P_claim_n = Q('101001', '000300')`

### How Other Players Make Claims

The non-agent players use a strategy that involves:
1. Assessing the situation by considering the total number of dice in play and their own roll.
2. Choosing a tactic at random, which can be bluffing, playing it safe (conservative), or being random.

- Bluffing might involve slight increases in the number of dice or their value.
- Playing conservatively involves making claims based on the most abundant dice in their hand.
- Being random introduces unpredictability with less rational claims.

### How Other Players Make Decisions

Other players use a mix of fixed and probabilistic decision rules:

- Fixed Decision Rule: Always doubt the maximum claim.
- Probabilistic Decision Rule: Use a calculated probability to inform decisions, with a mix of random chance (70%) and weighted decisions (30%) based on the probability calculated using the binomial probability mass function (PMF).

### Reward System

The reward system is designed to encourage the Agent to learn when to doubt effectively and make believable claims:

- Winning/losing a round by doubting: +/- 3 points
- Winning/losing a round by being doubted: +/- 1 point
- Winning/losing the entire game: +/- 5 points

### Update Q Table

The Q-table is updated as follows:

`Updated_Q(i, j) = Q(i, j) + α * reward(i, j) * (1 - γ * Q(i, j))`

where `α = 0.1` (learning rate) and `γ = 0.9`. This update rule adjusts the Agent's probability of doubting based on the outcome of its actions and the associated rewards.



## Contact
For any queries or contributions, feel free to contact [Your Contact Information].

## License
This project is licensed under [Your Chosen License].
