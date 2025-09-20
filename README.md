# Extreme Risks in Finance

## Description

This student project, developed as part of the module on **Random Numerical Simulation (SNA)** around Rare Events at École Polytechnique, focuses on modeling extreme risks in market finance. The project implements the **Black-Scholes model** for pricing European options, simulates portfolio value evolution using **geometric Brownian motion (GBM)**, and employs various rare event simulation techniques to estimate small probabilities (e.g., probabilities of losses above or below certain thresholds) and extreme quantiles, which correspond to **Value at Risk (VaR)** measures.

The notebook covers the following key implementations and analyses:

### Reproducibility Setup
- A `reproductivité` function to set a random seed (**42**) for consistent simulation results.

### Black-Scholes Model Implementation
- Functions `d_po` (**d1**) and `d_ne` (**d2**) for Black-Scholes parameters.
- `Call` and `Put` functions to compute prices of European call and put options at any time **t** before maturity **T**, handling maturity cases with intrinsic values.

### Portfolio Valuation
- `valeur_portefeuille` function to calculate the aggregate value of an uncovered portfolio as a linear combination of calls and puts weighted by arrays **alpha** (for calls) and **beta** (for puts) across multiple underlying assets.

### Simulation of Asset Trajectories and Portfolio Evolution
- `simulation` function:
  - Generates multiple (**I0=10** by default) GBM trajectories for underlying asset prices **S_t** over a discrete time grid (**N=1000** steps) from **0** to **T=1**.
  - Computes the portfolio value **V(t)** at each time step using the valuation function.
  - Plots:
    - **Left**: GBM trajectories of S(t) for each simulation.
    - **Right**: Evolution of V(t) (**red**) and cumulative loss/gain V(0) - V(t) (**black**).

### Rare Event Simulation and Risk Metrics
Focuses on estimating extreme risks using advanced simulation methods for rare events.
Implements multiple techniques to calculate:
- **Probabilities above or below thresholds**: Estimation of small probabilities **P(L > τ)** or **P(L < τ)**, where L represents portfolio losses or values, using methods efficient for rare events (e.g., probabilities on the order of **0.0001**).
- **Value at Risk (VaR)**: Extreme quantiles of the loss distribution, equivalent to VaR at confidence levels like **99.99%** (**alpha=0.0001**).

**Specific methods include:**
- **Particle-Based Quantile Estimation**: The `last_particle_quantile` function, likely a particle filter or multilevel splitting algorithm adapted for quantile estimation. It uses:
  - **n=1000** particles.
  - **n_tour=10** iterations or tours.
  - **rho=0.9** (possibly a tuning parameter for splitting factor or acceptance rate).
  - Adaptive levels **L_j** to progressively estimate the extreme quantile **q_hat**.
  - **Outputs**: Estimated quantile q_hat, number of iterations j_n, list of adaptive levels L_values, and a **95% confidence interval**.

**Other implied methods** (based on imports and context): Crude Monte Carlo simulations (possibly parallelized with Joblib), importance sampling, and density estimation using Gaussian KDE (`gaussian_kde`) for empirical distributions.

**Timing measurements** (using time module) to compare efficiency of methods.

**Visualizations** with Seaborn and Matplotlib enhancements (e.g., `AutoMinorLocator` for ticks).

### Portfolio Configurations and Analysis
Three portfolios tested with **I0=10** assets, **S0=100**, **K=100**, **sigma=0.5**:
- **Portfolio 1**: Long positions (**alpha=10**, **beta=5** for all assets).
- **Portfolio 2**: Short positions (**alpha=-10**, **beta=-5** for all assets).
- **Portfolio 3**: Mixed positions (short on first 5 assets, long on next 5).

**For each portfolio:**
- Runs the quantile estimation.
- **Prints**: Quantile estimate, iterations j_n, and 95% confidence interval.
- **Plots**: Adaptive levels L_j over iterations, with a dashed red line for the final quantile estimate (in a **3x1 subplot** figure).

### Additional Utilities
- **Parallel processing** with Joblib for efficient simulations.
- **Statistical tools** from SciPy (e.g., `norm` for normal CDF in Black-Scholes).

The project demonstrates how to handle rare events in finance, such as extreme portfolio losses, using simulation techniques that improve efficiency over naive Monte Carlo for small probabilities and high quantiles.

## Project Documentation

A detailed **project report** (PDF) is available in this repository, providing comprehensive documentation of:
- Theoretical background on rare event simulation
- Mathematical formulations and algorithms
- Implementation details and code architecture
- Results analysis and performance comparisons
- Conclusions and potential extensions

## Authors

- **Dzikouk Frank Dilane** (2nd-year student at École Polytechnique, GitHub: @FrankDZIKOUK)
- **Christel Astride Mallo Poundi** (2nd-year student at École Polytechnique, GitHub: @astride11)

## Technologies Used

- **Python 3**
- **Libraries**: NumPy (arrays, math), Matplotlib (plotting), SciPy (stats, Gaussian KDE), Joblib (parallelization), Seaborn (visualizations), and time (timing).

## Installation

Clone the repository:
```bash
git clone https://github.com/YourUsername/Extreme-Risks-in-Finance.git
```

Install the dependencies:
```bash
pip install numpy matplotlib scipy joblib seaborn
```

## Usage

1. Open the `Notebook.ipynb` file in **Jupyter Notebook** or **JupyterLab**.
2. Run the cells sequentially:
   - Imports and reproducibility setup.
   - Black-Scholes and portfolio functions.
   - Simulation function for evolution and plots.
   - Parameters and portfolio definitions.
   - Loop to estimate quantiles/VaR for each portfolio using `last_particle_quantile` and other methods.
   - Display results (prints and plots).

3. Customize parameters (e.g., `alpha_quantile=0.0001` for extreme VaR) or add thresholds for probability estimations.
4. Enable reproducibility with `will = True`.

### Example outputs
- **GBM trajectories** and portfolio value plots.
- **Quantile/VaR estimates** with confidence intervals.
- **Adaptive level plots** for rare event methods.

## Acknowledgments

- Thanks to **@astride11** for collaboration.
- Special thanks to instructors at **École Polytechnique** for guidance on rare event techniques like **multilevel splitting** and **particle methods**.
