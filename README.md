# R Code for "Evaluation of Asymptotic Hypothesis Tests in the Unit-Lindley Regression Model"

[![Language](https://img.shields.io/badge/Language-R-blue.svg)](https://www.r-project.org/)

This repository contains all R code necessary to reproduce the findings of the undergraduate thesis:

> **"Evaluation of Asymptotic Hypothesis Tests in the Unit-Lindley Regression Model: A Simulation Study and Application"**

* **Author:** Vinicius Ferreira Amorim Santana
* **Institution:** Universidade Federal de Goiás (UFG)
* **Advisor:** Prof. Dr. Tatiane Ferreira do Nascimento Melo da Silva

## Project Goal

The project's objective is to evaluate and compare the finite-sample performance of five asymptotic hypothesis tests (Wald, Corrected Wald, Likelihood Ratio, Score, and Gradient) within the Unit-Lindley (UL) regression model framework.

This is achieved through two main components:

1.  **A Monte Carlo Simulation Study:** To assess the empirical rejection rates (Type I error) and bias of the tests under various controlled scenarios (different sample sizes, number of parameters, etc.).
2.  **A Real-Data Application:** To illustrate the practical implications of the tests on a real-world dataset ($n=27$), modeling the Brazilian Education Index (IDEB) as a function of GDP per capita.

## Repository Contents

This repository contains two primary scripts, corresponding to the two parts of the thesis:

1.  `simulations.R` (based on `TCC_Monte_Carlo.rmd`): The script to run the Monte Carlo simulation study.
2.  `application.R` (based on `TCC_IDEB_PIB.rmd`): The script to run the real-data application.

---

### Part 1: Monte Carlo Simulation (`simulations.R`)

This script (based on `Codigo_IC.pdf`) contains the code to generate the simulation results presented in the thesis (Tables 1-4).

**Methodology:**
* Loops over a range of sample sizes: $n \in \{10, 20, 30, 40, 50\}$.
* Loops over model complexity (number of parameters): $p \in \{2, 3, 4, 5, 6\}$.
* Loops over the number of parameters under test: $q \in \{1, 2\}$.
* For each scenario, it performs **10,000 Monte Carlo replicates** (Note: the script variable `REP` is set to 100,000, but the thesis results were generated using 10,000 ).
* In each replicate, it:
    1.  Generates data from the Unit-Lindley model under the null hypothesis ($H_0$) .
    2.  Fits the unrestricted model (MLE) via `optim()` .
    3.  Fits the restricted model (under $H_0$) .
    4.  Calculates all five test statistics: Wald (W), Corrected Wald (Wc), Likelihood Ratio (LR) , Score (E), and Gradient (G).
    5.  Stores the results.
* After the loop, it calculates the empirical rejection rates (e.g., `Taxa_W_5`) by comparing the test statistics against the quantiles of the Chi-squared distribution.
* The script also includes code to **calculate the bias** of these rejection rates and hard-coded dataframes to quickly reproduce the bias tables (Tables 3 and 4) from the TCC without re-running the full simulation.

---

### Part 2: Real-Data Application (`application.R`)

This script (based on `Codigo.pdf`) contains the code for the real-data analysis (Chapter 3.3).

**Methodology:**
1.  **Data Acquisition:** Queries the public dataset repository `basedosdados` to fetch IDEB data (from INEP) and GDP/Population data (from IBGE) for the 27 Brazilian Federal Units for 2019.
2.  **Data Preprocessing:** Cleans and transforms the data (IDEB to (0,1), GDP per capita scaled by 1000) for numerical stability.
3.  **Model Implementation:** Defines the core functions for the Unit-Lindley regression model, including:
    * Log-Likelihood functions .
    * Score Vector function .
    * Fisher Information Matrix function .
    * Second-Order Covariance Matrix (for Wc).
4.  **Model Fitting:** Fits the UL model using `optim()` to the IDEB vs. GDP data .
5.  **Hypothesis Testing:** Calculates all five test statistics (W, Wc, LR, E, G) for the hypothesis $H_0: \beta_1 = 0$ .
6.  **Results Generation:** Generates the final tables (using `knitr::kable`) showing the model fit and the comparison of the hypothesis tests.

## Requirements

The code is written in R. To run it, you will need the following packages, which are automatically installed by the `pacman` package manager:

**For `simulations.R`:**
* `pacman`
* `xtable` 
* `MASS` 
* `numDeriv` 
* `progress` (for the progress bar) 
* `parallel` 

**For `application.R`:**
* `pacman`
* `basedosdados` (for data query)
* `knitr` (for table formatting) 
* `DHARMA` 
* `boot` 

You will also need to set up a billing ID for Google Cloud to use the `basedosdados` package, as described in their documentation. The `application.R` script requires you to insert your own ID at this line:

```R
# Defina seu ID de projeto do Google Cloud
set_billing_id("seu-id-de-projeto-aqui")
```

## How to Run

1.  Clone this repository.
2.  **To run the application:**
    * Set up your Google Cloud Billing ID as instructed [here](https://basedosdados.github.io/mais/access_data_bq/#configuracao-de-projeto-no-google-cloud-platform-gcp).
    * Open `application.R`.
    * Replace the placeholder `meu-tcc-vinicius` with your own billing ID.
    * Run the entire script. The console will print the final tables.
3.  **To run the simulation:**
    * Open `simulations.R`.
    * **Warning:** This is a computationally intensive script and may take several hours or days to run, depending on your hardware.
    * You can adjust the parameters `REP`, `p`, `q`, and `tamanhos_amostrais` at the top of the file to run specific scenarios.
    * Run the script. The empirical rejection rates will be printed to the console upon completion.

## How to Cite

If you use this code or the findings from this work, please cite the original thesis:

SANTANA, V. F. A. "Avaliação de Testes de Hipóteses Assintóticos no Modelo de Regressão Lindley-Unitária: 
Um Estudo de Simulação e aplicação". 
Trabalho de Conclusão de Curso (Bacharelado em Estatística) - 
Instituto de Matemática e Estatística, Universidade Federal de Goiás, Goiânia, 2025.


```bibtex
@misc{SANTANA2025TCC,
  author  = {Santana, Vinicius Ferreira Amorim},
  title   = {Avaliação de Testes de Hipóteses Assintóticos no Modelo de Regressão Lindley-Unitária: Um Estudo de Simulação e aplicação},
  year    = {2025},
  month   = {nov},
  school  = {Universidade Federal de Goiás, Instituto de Matemática e Estatística},
  type    = {Trabalho de Conclusão de Curso (Bacharelado em Estatística)},
  address = {Goiânia}
}
