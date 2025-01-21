# Starter: Multi-Echelon Supply Chain Optimization and Pathfinding using NetworkX

This repository demonstrates a **graph-based approach** to modeling, analyzing, and optimizing **multi-echelon supply chains** using **NetworkX** and **PuLP**. It also includes **pathfinding** solvers (BFS, Dijkstra, A*, MCTS) for illustrative or heuristic use-cases.

## Contents

1. [Overview](#overview)  
2. [Features](#features)  
3. [Dependencies & Installation](#dependencies--installation)  
4. [Folder Structure](#folder-structure)  
5. [Usage](#usage)  
6. [Example Workflow](#example-workflow)  
7. [Extending the Project](#extending-the-project)  
8. [License](#license)

---

## Overview

**Key Goal**: Provide a flexible, modular framework for supply chain practitioners (or anyone dealing with directed graphs) to:

- Construct a **directed multi-echelon supply chain** as a network of factories, distribution centers, warehouses, and retailers.  
- Use **pathfinding solvers** (BFS, Dijkstra, A*, MCTS) to explore potential routes or heuristics.  
- **Optimize flows** using a **PuLP**-based linear program with supply/demand constraints, capacity limits, and cost/profit objectives.  
- **Visualize** the resulting network with advanced node-coloring.  
- **Export** solutions and charts to **PDF** for reporting.

It’s intentionally **modular** so you can **swap in** different solvers, attach sub-networks to nodes, or add advanced constraints.

---

## Features

1. **Node/Edge Classes**  
   - Each node can store custom constraints, a solver, or an objective function.  
   - Each edge can have a transform, weight, or capacity (tracked in an external DataFrame if you prefer).

2. **Graph Class**  
   - Handles node/edge storage, **simulate()** (a simple forward pass) and **solve()** (calls the attached solver).

3. **Solvers**  
   - **BFS**: Finds a path from start to goal in an unweighted sense.  
   - **Dijkstra**: Finds the shortest weighted path.  
   - **A***: Pathfinding with a heuristic function.  
   - **MCTS**: A simple Monte Carlo Tree Search approach that tries random paths and keeps the best.  
   - **PuLP Solver**: For linear programs, e.g., supply/demand flows to maximize profit minus cost.

4. **Visualization**  
   - **NetworkX** + **Matplotlib** to draw the graph, color-coded by node type (e.g. Factory, DC, Warehouse, Retailer).  
   - Optional edge labels for weight/cost/capacity.  
   - Ability to export the figure to PDF.

5. **PDF Exports**  
   - Text-only solution export (status, objective value, flows) with **ReportLab**.  
   - Combined graph + solution text in a single PDF.

6. **Comprehensive Example**  
   - Demonstrates building a **multi-echelon** network, running BFS/Dijkstra/A*/MCTS, then performing a PuLP optimization.  
   - Prints a comparison table of path costs and the final solution flows.

---

## Dependencies & Installation

- **Python 3.8+** (though other versions may work)
- **NetworkX**  
- **Matplotlib**  
- **Pandas**  
- **PuLP**  
- **ReportLab** (optional, for PDF exports)

**Install dependencies** via pip (example):
```bash
pip install networkx matplotlib numpy scipy pandas pulp reportlab uuid
```

If you’re on **Apple Silicon** (M1/2/3...) and get errors with CBC, install a native CBC solver:
```bash
brew tap coin-or-tools/coinor
brew install cbc
```
Then pass the path (e.g. `/opt/homebrew/bin/cbc`) to the `PulpSolver` constructor.

---

## Folder Structure

A possible directory layout could be:

```
my_supply_chain_project/
│
├─ requirements.txt            # If you want to pin dependencies
├─ README.md                   # This README file
├─ solver_base.py              # Contains the Solver classes: BFS, Dijkstra, A*, MCTS
├─ pulp_solver.py              # Contains the PulpSolver class
├─ node_edge.py                # Node, Edge classes
├─ custom_graph.py             # Graph class
├─ utils.py                    # Helpers (compute_path_cost, PDF exports, etc.)
├─ main_demo.ipynb             # Jupyter notebook or main script showing usage
└─ ...
```

*(Adjust filenames to suit your preference.)*

---

### Command-Line

Run the jupyter notebook in order

The script builds the multi-echelon network, runs all solvers, prints results, and optionally exports graphs/solutions to PDF.

### Jupyter Notebook

If you have the notebook version, open it:
```bash
jupyter notebook networkx-scm-starter.ipynb
```
Run each cell to see the supply chain get built, solvers run, and results displayed.

---

## Example Workflow

1. **Construct Your Graph**  
   - Create nodes for factories, DCs, warehouses, retailers.  
   - Add edges representing possible routes, with weights (cost, distance, etc.).

2. **Attach Solvers**  
   - For pathfinding: BFS, Dijkstra, A*, MCTS (for demonstration or heuristic route checks).  
   - For optimization: Use `PulpSolver` with `build_problem()` passing supply, demand, cost, capacity data.

3. **Run the Solvers**  
   - **BFS, Dijkstra, A*, MCTS** from a chosen start node to end node.  
   - **PuLP** for linear optimization of flows.

4. **View & Compare Results**  
   - **Print** or **log** the path or flows.  
   - **Visualize** the graph with color-coded nodes.  
   - **Export** results to PDF for easy sharing or archiving.

---

## Extending the Project

- **Integer or Binary Flows**: Switch flow variables to `pulp.LpInteger` or `pulp.LpBinary` for discrete flows or yes/no route usage.  
- **Multi-Commodity**: Track multiple products each with its own supply/demand.  
- **Multi-Period**: Introduce time-based constraints, lead times, or inventory.  
- **Sub-Graphs**: Nest smaller graphs under certain nodes if you want local optimizations.  
- **Additional Constraints**: Service-level constraints, emission targets, etc.  

As a general framework, it’s easy to **inject** new constraints or **define** new objective functions. Just adapt the `PulpSolver.build_problem()` method or add a different solver.

---

## License

This project is open-sourced under the [MIT License](https://opensource.org/licenses/MIT). You’re free to copy, modify, and distribute as needed. Feel free to contribute enhancements, bug fixes, or new solvers!

---

**Happy Optimizing!** If you have any questions, feature requests, or run into issues with solver setup, feel free to open an Issue or pull request.