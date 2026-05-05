# CFD-Validation-using-Finite-Element-Method
Validation of GPU-accelerated Multiple-Relaxation-Time Lattice Boltzmann Method (MRT-LBM) simulations for convective flows in porous media using COMSOL Multiphysics (Finite Element Method). Based on the 2018 study by Molla et al.
# COMSOL Validation of MRT-LBM for Convective Flows in Porous Media

This repository contains the COMSOL Multiphysics validation models, results, and documentation for simulating natural convection in fluid-saturated porous enclosures. 

This project was developed as a Computational Fluid Dynamics (CFD) study to independently verify the numerical methodology and results presented in the paper:
> **GPU Accelerated Multiple-Relaxation-Time Lattice Boltzmann Simulation of Convective Flows in a Porous Media** by Molla et al. (2018), *Frontiers in Mechanical Engineering*.

## 📌 Project Overview

Accurate numerical simulation of convective flows in porous media is critical for applications like electronic cooling, geothermal energy systems, and thermal insulation. While the reference study utilized a highly efficient GPU-accelerated double MRT-LBM framework (CUDA C), this project successfully reproduces those complex flow physics using the **Finite Element Method (FEM)** within COMSOL Multiphysics.

### Key Objectives:
1. **Pure Fluid Natural Convection:** Replicate benchmark results (De Vahl Davis, 1983) for Rayleigh numbers ($Ra$) from $10^3$ to $10^6$.
2. **Porous Media Natural Convection:** Validate heat transfer behavior using the **Brinkman-Forchheimer extended Darcy model** across a wide parameter space:
   * Rayleigh Numbers ($Ra$): $10^3$ to $10^{10}$
   * Darcy Numbers ($Da$): $10^{-2}$ to $10^{-7}$
   * Porosity ($\epsilon$): $0.4$ and $0.6$

## ⚙️ Methodology & Governing Equations

The 2D square cavity ($H=L=1$) is modeled using laminar, incompressible Navier-Stokes equations coupled with the heat equation. The Boussinesq approximation is applied for buoyancy.

For the porous media cases, the **Brinkman-Forchheimer** model is implemented as a volume force $\mathbf{F}$, accounting for linear (Darcy) and non-linear (Forchheimer) drag:
$$ \mathbf{F} = -\frac{\epsilon\nu}{K}\mathbf{u} - \frac{1.75}{\sqrt{150\epsilon K}}|\mathbf{u}|\mathbf{u} + \epsilon G $$
Where $K$ is the permeability ($Da \cdot H^2$) and $G$ is the thermal buoyancy term.

## 📊 Key Results

### 1. Pure Fluid Validation
* **Qualitative:** Streamlines and isotherms perfectly match the De Vahl Davis benchmarks, accurately capturing the transition from symmetric conduction dominance at $Ra=10^3$ to strong boundary layer convection at $Ra=10^6$.
* **Quantitative:** Average Nusselt numbers ($\overline{Nu}$) show systematic agreement within ~5-10% of the highly specialized benchmark data.

### 2. Porous Media Validation
* **Flow Regimes:** The COMSOL model successfully distinguishes between the **Darcy regime** ($Da \le 10^{-6}$), where fluid motion is heavily suppressed resulting in linear isotherms, and the **non-Darcy regime** ($Da \ge 10^{-4}$), where distinct convective rolls form.
* **Heat Transfer:** The calculated average Nusselt numbers ($\overline{Nu}$) are in excellent agreement with the GPU MRT-LBM data from Molla et al. (2018) across all tested porosities and Rayleigh/Darcy numbers.

*Note: The COMSOL stationary solver diverges at $Ra \ge 10^7$ due to the physical transition of the flow from steady laminar to unsteady/periodic, highlighting the inherent advantage of the explicit time-marching MRT-LBM approach for highly unstable flow regimes.*

## 📂 Repository Structure

* `/COMSOL_Models/` - Contains the `.mph` files for both pure fluid and porous media simulations.
* `/Results_Data/` - Spreadsheets containing the extracted Average Nusselt numbers, local Nusselt numbers, and velocity profiles.
* `/Figures/` - High-resolution plots comparing COMSOL isotherms and streamlines against the reference benchmarks.
* `/Docs/` - The final project report (`CFD_Validation_Project.pdf`).

## 🚀 How to Run

1. Open COMSOL Multiphysics (version X.X or newer recommended).
2. Navigate to the `/COMSOL_Models/` directory and open the desired `.mph` file.
3. In the **Global Definitions > Parameters** node, adjust the Rayleigh ($Ra$), Darcy ($Da$), and Porosity ($\epsilon$) values as needed.
4. Click **Compute** under the Study node. *(Note: For $Ra \ge 10^7$, a time-dependent study is required due to flow instabilities).*

## 👥 Authors
* **Neyaz Ahmed** 
* **Md Obaidullah** 

## 📖 References
1. Molla, M. M., Haque, M. J., Khan, M. A. I., & Saha, S. C. (2018). GPU accelerated multiple-relaxation-time lattice Boltzmann simulation of convective flows in a porous media. *Frontiers in Mechanical Engineering*, 4, 15.
2. De Vahl Davis, G. (1983). Natural convection of air in a square cavity: a benchmark numerical solution. *International Journal for Numerical Methods in Fluids*, 3(3), 249-264.
