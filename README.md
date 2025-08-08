# vanguard-quantum-portfolio-optimization
A quantum-enhanced solution for the Vanguard Portfolio Optimization Challenge, developed for the Womanium + WISER Quantum 2025 program using a QAOA-based algorithm.

# Vanguard Portfolio Optimization 

**Participants:** Sadiya Ansari, Rudraksh Sharma, VU Binh            
**Submission for:** Womanium + WISER Quantum 2025

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python Version](https://img.shields.io/badge/python-3.9%2B-blue.svg)](https://www.python.org/downloads/)

---

## 🚀 Challenge Overview

This project tackles the Vanguard Portfolio Optimization Challenge by developing a quantum-enhanced solution. The goal is to solve a complex, real-world portfolio construction problem with a quadratic objective and multiple constraints. We aim to evaluate the performance of a quantum optimization algorithm against a classical benchmark.

---

## 🎯 Our Approach

We designed a modular workflow to translate the business problem into a solvable quantum format and interpret the results. Our methodology is as follows:

1.  **Mathematical Conversion**: We transformed the original constrained problem into an unconstrained QUBO (Quadratic Unconstrained Binary Optimization) model. To handle inequality constraints efficiently without introducing extra qubits, we implemented a **slack-free unbalanced penalization** method.
2.  **Quantum Solution**: We employed the **Quantum Approximate Optimization Algorithm (QAOA)** using the high-level **OpenQAOA** library. We chose QAOA for its suitability to combinatorial optimization problems.
3.  **Classical Validation**: To benchmark our quantum solution, we solved the same QUBO problem using a classical solver, which provides a strong heuristic solution.
4.  **Comparative Analysis**: We rigorously compared the quantum and classical solutions based on the final cost function value, runtime, and performance as the problem size (number of assets) increases.

---

## 📂 Repository Structure

```
.
├── .gitignore
├── LICENSE
├── README.md
├── requirements.txt
├── data/
│   └── problem_instance_15_assets.py
├── notebooks/
│   └── main_demonstration.ipynb
└── src/
    ├── __init__.py
    ├── classical_solver.py
    ├── problem_converter.py
    └── quantum_solver.py
```

---

## ⚙️ How to Run

To replicate our results, please follow these steps:

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/](https://github.com/)sadieea/vanguard-quantum-portfolio-optimization.git
    cd vanguard-quantum-portfolio-optimization
    ```

2.  **Create and activate a virtual environment:**
    *Using `venv` (recommended):*
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows use `venv\Scripts\activate`
    ```
    *Alternatively, using `conda`:*
    ```bash
    conda create -n vanguard-qaoa python=3.10
    conda activate vanguard-qaoa
    ```

3.  **Install the required dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Run the demonstration notebook:**
    Launch Jupyter and run the `main_demonstration.ipynb` notebook located in the `/notebooks` directory.
    ```bash
    jupyter notebook notebooks/main_demonstration.ipynb
    ```

---

## 📝 Mathematical Formulation

### Original Problem
The model seeks to find an optimal portfolio by minimizing a quadratic objective function subject to several linear constraints.
* **Decision Variable**: $$y_c \in \{0, 1\}$$, indicating if bond $$c$$ is in the portfolio.
* **Objective Function**: Minimize the tracking error against target characteristics.             
    $$\min \sum_{l \in L}\sum_{j \in J}\rho_j\left(\sum_{c \in K_l} \beta_{c,j}x_c-K^{target}_{l,j}\right)^2$$
* **Main Constraints**: The model is subject to multiple guardrails, including a maximum number of bonds in the basket ($\sum y_c \le N$) and limits on residual cash flow.

### QUBO Formulation with Unbalanced Penalization
We convert the problem into an unconstrained QUBO model by adding penalty terms for each constraint. The total cost function is $$H = H_{objective} + H_{constraints}$$.        
For an inequality constraint like $$\sum y_c \le N$$, the unbalanced penalty term is:          
$$P = -\lambda^{(0)}\left(N - \sum_{c\in C} y_c\right) + \lambda^{(1)}\left(N - \sum_{c\in C} y_c\right)^2$$         
This penalizes violations quadratically while adding minimal energy in the feasible region. The full QUBO model includes seven such penalty terms, as derived in our source code.

### Ising Hamiltonian
The final QUBO model, which uses binary variables $$y_c \in \{0, 1\}$$, is then converted to an Ising Hamiltonian using the standard mapping $$y_c = (1 - z_c) / 2$$, where $$z_c \in \{-1, 1\}$$. This results in a Hamiltonian that OpenQAOA can solve:   
$$H_{total} = \text{const}^{tot} + \sum_c h^{tot}_c z_c + \sum_{c < c'} J^{tot}_{cc'} z_c z_{c'}$$

---

## 💡 Solution & Results


---

## ⚖️ Conclusion

