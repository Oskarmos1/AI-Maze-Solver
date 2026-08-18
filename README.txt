AI Maze Solver EPQ Artefact

📖 Overview

This project is an Extended Project Qualification (EPQ) artefact developed as a proof-of-concept for an autonomous maze-learning solution. The project explores the intersection of Discrete Mathematics and Artificial Intelligence, specifically using Reinforcement Learning to navigate complex environments.

The inspiration for this project came from a robotics competition involving the developer's younger brother, Aleks, which required a solution for memorizing and solving mazes without a top-down perspective.

🚀 Quick Start & Setup

Prerequisites

Python 3.x

matplotlib: For real-time plotting and data visualization.

networkx: For handling discrete mathematical graph structures and planarity checks.

Installation

Clone the repository and install the necessary dependencies via pip:



# Clone the repository

git clone 

cd ai-maze-solver

# Install dependencies

pip install matplotlib networkx

Running the Solver

The main execution script generates a random maze, benchmarks it using Dijkstra's algorithm, and then initiates the AI training process.



# Run the solver

python main.py

🛠️ Core Features

1. Robust Maze Generation

The system utilizes a multi-stage process to create realistic, solvable mazes:

Connectivity Check: Uses Breadth-First Search (BFS) to ensure the randomly generated adjacency matrix represents a fully connected graph.

Structural Optimization: Implements Prim’s Algorithm to produce a Minimum Spanning Tree (MST), removing unnecessary arcs to create a maze-like skeleton.

Complexity & Realism: Introduces a 12.5% chance to re-add removed arcs, creating loops and cycles to increase difficulty.

Planarity Check: Leverages networkx to ensure the maze can be drawn without overlapping corridors, mimicking a real-world layout.

2. Deterministic Benchmarking (Dijkstra’s)

To evaluate the AI's performance, the program first solves the maze mathematically using Dijkstra’s Algorithm. This provides the absolute shortest path and a performance benchmark for the reinforcement learning agent.



3. Reinforcement Learning Agent

The AIMazeSolver class implements a Tabular Q-Learning model.

Epsilon-Greedy Policy: The agent balances "exploration" (taking random steps to discover the maze) and "exploitation" (using its memory to find the exit).

Bellman Equation: The agent updates its internal "Q-table" after every step based on immediate rewards and future potential.

Punishment System: The agent is penalized (negative rewards) for longer paths, incentivizing it to converge on the optimal route.

Convergence: Training automatically terminates once the agent consistently finds the same path for 20 consecutive episodes.

🎨 Visualization Legend

The UI uses matplotlib to provide an educational view of the algorithms in action:

🟡 Yellow Nodes: Represent fixed nodes during the forward pass of Dijkstra's algorithm.

🟢 Green Paths: Indicate the agent's exploration steps during the AI training episodes.

🔴 Red Paths: Highlight the final optimal path found by either the mathematical solver or the trained AI.

🔵 Light Blue Nodes: Represent unexplored or non-fixed areas of the maze.

📂 Repository Structure

main.py: The entry point that initializes the Maze and AIMazeSolver.

Maze class: Handles random graph generation, MST conversion, and Dijkstra's algorithm.

AIMazeSolver class: Contains the Q-table, training logic, and the Bellman equation implementation.

Visualisation: Global subroutines for rendering the graph using NetworkX and Matplotlib.

📝 Credits

Developed by Oskar (2025-2026) as part of an Extended Project Qualification. This project serves as a technical demonstration of AI foundational concepts for A-Level Computer Science and Further Mathematics studies.