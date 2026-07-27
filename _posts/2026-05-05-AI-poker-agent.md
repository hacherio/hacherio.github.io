--- 
title: Teaching a Machine to Bluff - AI Poker Agent Limit Texas Hold'em (UMass Amherst COMPSCI 683)
date: 2026-05-05 06:00:00 -0400
categories: [Projects, Agent]
tags: [learning, artificial-intelligence, game theory, machine-learning]
---

Poker is one of those games that can be strategic, even if it's a luck-based type of gambling formatted games. When it comes to decision making under uncertainty, you wouldn't be able to know th opponent's hand or the next card, yet there are few players that are good at consistently winning by reading patterns and understanding the risk and rewards through actions. UMass Amherst offers a COMPSCI 683 course, artificial intelligence, within the realms of game theory. We were given a team-based final project challenge, in which my team and I created an agent called `RaisedPlayer` that plays heads-up (2 player) Limit texas Hold'em by combinging adversarial search with learning evaluation functions. Here is the project [link in rough draft](https://github.com/hacherio/AI-Poker-Agent), though our MAIN repository project is kept in private.

This project is done in a team of 4 in developing a competitive AI agent for 2-player Limit Texas Hold'em poker, built for COMPSCI 683 Artificial Intelligence course tournament. The agent uses `RaisedPlayer` in `raise_player.py` combines adversarial search with real-time opponent modeling and learned linear function.

The agent has three actionable decision before playing: fold, call, or raise. The algorithm goes through a layered pipeline of decisions where there's a bluffing module for profitable opportunities, a search engine (minimax with alpha-beta pruning), and learned linear evalutation function:


| Component            | Description                                                                             |
| -------------------- | --------------------------------------------------------------------------------------- |
| Minimax + alpha-beta | Depth-3 adversarial search over abstract state space                                    |
| Profile-weighted EV  | Opponent nodes use a distribution over actions, not pure min                            |
| Semi-bluff           | Raise drawing hands on flop and turn to extract 2                                       |
| Reverse Bluff        | Call hands against agressive opponents to trap 2                                        |
| Opponent Modeling    | Classify opponent as either aggressive, passive, balanced depending on action history 2 |
| Made hand evaluator  | Scores postflop hand strength from available cards                                      |
| Linear Evaluator     | Score minimax leaf nodes with RL-trained weights                                        |

However, since our team decide to split the work through differing algorithm, we constructed four agents and a controlled selection space over them. One of our agent, PureMCAgent, utilizes 265M simulation pre-flop table and time-bounded post flop Monte Carlo. MCCFRAgent is a 2 Million iteration external sampling MCCFR strategy table over 8-bucket card abstraction, augmented at runtime with Bayesian opponent-exploit layer. HybridAgent is a depth-3 alpha-beta minimax search with 14 feature linear leaf evluator and deterministic semi-bluff, with weighted trained online via Q-Learning. GauntletAgent extend the HybridAgent with 50 simulation MC hand-strength estimation, a bluff pipeline.

![Offline budget](/assets/img/budget.png)

We model the game through the hold' em format: two players, four betting streets, and three legal actions per turn (fold/call/raise). All agents are experimented in a test enviroment using standard pypokerengine runner with two agents and an initial stack of 1,000 chips. Ultimately, the GauntletAgent wins as it tuned weights across four opponent classes simultaneously. 

![Results](/assets/img/results.png)

![Graph](/assets/img/resultsgraph.png)

If you are interested in reading more indepth of this project, download the link below:

[Download AI Poker research (PDF)](/assets/img/683_AI_report.pdf)
