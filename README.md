# Parameter Inference and Modelling of the Prophylactic Efficacy of Islatravir against HIV-1

This framework was used to infer the pharmacokinetic (PK) and pharmacodynamic (PD) properties of Islatravir (ISL) and to assess its potential as new long-term application for HIV-1 treatment and prevention strategies. The here introduced procedure was implemented as methodical approach in the context of the eponymic master thesis. 

The implementation can be found in the folder **Code**, the used clinical data is provided in the folder **Data**. For testing the scripts, set the number of maximal counts *maxcount* to a low number in each program, such as *maxcount = 10*. To achieve stable results, *maxcount* has to be set larger e.g. *maxcount = 100*, causing a longer run time.

Based on the structure of this repository, the workflow can be described by following parts:

**1_PK**
**1_plasma**: PK Plasma Modelling

Prior to the model fitting and parameter estimation process, the feasible parameter space was manually evaluated and restricted with respect to the RSS value. This step is necessary to ensure a bearable run time for the subsequent optimization. A visualization is given in → **_plot_findboundaries_**, the results are stored in the respective directory.

In → **_pk_plasma_** the identified boundaries for the parameters were used to randomly sample within this region and to choose the best fitting model to describe the clinical data, in terms of minimal AIC and RSS values.

**1_intracellular**

In this section the resulting parameters from 1. were taken and combined with intracellular data of Islatravir to determine the transmission kinetics between the central and intracellular compartment. The kernel of the program → **_pk_intra_linear_** is an ODE-system based on linear kinetics. The main part of the program → **_pk_intra_mmk_** consists of the corresponding ODE-system describing Michaelis-Menten kinetics. 

Furthermore, the order of the respective elimination rate from the intracellular compartment was observed by calculating the slope of the concentration-time data in program → **_get_slope_**, and some re-adjustment on the parameters was done in → **_estimate_vmax, estimate_ka_**.  As an intermediate step, the resulting PK model was validated on unseen data in program → **_matthews2018_**.

**2_PD**: Link to HIV-1 Dynamics

In program → **_steadystate_analysis_** a simulation of the course of an infection was made to explore the steady states of uninfected and infected cells and viruses. When this is done, in program → _**pd_optimize_** the IC50 value of Islatravir can be estimated.

**3_analysis**: Analysis and visualization of Islatravir’s properties

The final analysis consisted in simulating and understanding the clinically relevant mechanism of action (MOA) of the drug, including
→ **_ISLTP_single_dose_**, where concentration-time plots are constructed,
→ **_ISLTP_multiple_doses_**: creates plots for different dosing regimens
→ **_viral_load_**: the viral load over time for different doses is plotted
→ **_extrande_simulations, prophylactic_efficacy_ISL_**: assessing the prophylactic efficacy for different settings.

**4_simulation algorithms**: Simulation of the extinction/infection probability

In → **_compare_** an exact analytical algorithm of a Markov Jump Model and the stochastic Extrande algorithm was implemented and compared to compute the extinction and infection probabilities of the respective state of the system.

