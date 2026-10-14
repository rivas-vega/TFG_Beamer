# Mathematical Foundations of Energy-Based Models and Joint Embedding Architectures: From Representation Collapse to World Models

[![Thesis: Mathematics](https://img.shields.io/badge/Degree-B.S.%20in%20Mathematics-blue.svg)](https://www.us.es/)
[![Institution: Univ. de Sevilla](https://img.shields.io/badge/Institution-Univ.%20de%20Sevilla-red.svg)](https://matematicas.us.es/)
[![Supervised by Prof. Sánchez Karhunen](https://img.shields.io/badge/Advisor-Prof.%20E.%20S%C3%A1nchez%20Karhunen-orange.svg)]()
[![Pages: 100+](https://img.shields.io/badge/Monograph-100%2B%20Pages-green.svg)]()

> **Spanish Title:** *Fundamentos Matemáticos de los Energy-Based Models y las Joint Embedding Architectures: Del Representation Collapse a los World Models*  
> **Author:** Pedro Rivas Vega (`rivas-vega`)  
> **Supervisor:** Prof. Eduardo Sánchez Karhunen  
> **Department:** Departamento de Matemática Aplicada II, Universidad de Sevilla  
> **Grade / Distinction:** Highest Honors (Matrícula de Honor candidate)

---

## 📄 Primary Documents

- 📘 **[Complete Research Monograph (PDF)](./TFG_PDF_LATEX_PROJECT/main.pdf)** (100+ pages, complete mathematical proofs, figures, and LaTeX sources)
- 🖥️ **[Defense Presentation Slides (Beamer PDF)](./presentacion_proyecto/presentacion.pdf)** (64 defense slides)
- 📝 **[Oral Defense Script (guion.md)](./presentacion_proyecto/guion.md)** (Technical presentation narrative)

---

## 🎯 Executive Summary & Mathematical Thesis

This monograph provides a rigorous mathematical treatise on self-supervised representation learning, analyzing how non-generative architectures infer high-dimensional dependencies without catastrophic collapse. We trace the theoretical continuum from **Energy-Based Models (EBMs)** and **Jaynes' Principle of Maximum Entropy** to Yann LeCun's **Joint Embedding Predictive Architecture (JEPA)** and latent-space **World Models**.

A fundamental focus is given to the mathematical prevention of **representation collapse** $\left(f_\theta(x) \equiv c\right)$, proving why naive Siamese networks collapse, establishing the exponential sample complexity of negative-sampling contrastive methods in high dimensions, and analyzing modern isotropic regularizers based on empirical characteristic functions and the **Cramér-Wold device**.

```
                ┌────────────────────────────────────────────────────────┐
                │          Energy-Based Models (EBMs) & Gibbs            │
                │        P_β(x) = e^{-β E(x)} / Z_β, Jaynes MaxEnt       │
                └──────────────────────────┬─────────────────────────────┘
                                           │
                        ┌──────────────────┴──────────────────┐
                        ▼                                     ▼
         ┌──────────────────────────────┐      ┌──────────────────────────────┐
         │     Contrastive Methods      │      │   Non-Contrastive Methods    │
         │   InfoNCE, Negative Pairs    │      │  Barlow Twins, VICReg, JEPA  │
         │  O(e^d) covering bound risk  │      │  Information-theoretic decorr│
         └──────────────────────────────┘      └──────────────┬───────────────┘
                                                              │
                                            ┌─────────────────┴─────────────────┐
                                            ▼                                   ▼
                             ┌──────────────────────────────┐    ┌──────────────────────────────┐
                             │       VICReg Decoupling      │    │     SIGReg & Cramér-Wold     │
                             │  Variance, Invariance, Cov   │    │ Empirical char functions in  │
                             │  non-adversarial stabilization│   │ random 1D projections S^{d-1}│
                             └──────────────────────────────┘    └──────────────────────────────┘
```

---

## 📐 Chapter-by-Chapter Theoretical Overview

### Chapter 1: The Self-Supervised Paradigm & The Problem of Collapse
- Mathematical formulation of unsupervised structure discovery in high-dimensional measure spaces $(\mathcal{X}, \mu)$.
- Formal definition of the trivial representation manifold: $f_\theta: \mathcal{X} \to \mathcal{Z}$ with $\dim(\mathrm{Im}(f_\theta)) = 0$.

### Chapter 2: Energy-Based Models (EBMs) & Statistical Physics Foundations
- Energy functions $E_\theta(x, y)$ as unnormalized scalar compatibilities.
- The Gibbs-Boltzmann distribution: $p_\beta(x) = \frac{e^{-\beta E(x)}}{\int e^{-\beta E(\tilde{x})} d\tilde{x}}$.
- Derivation via **Jaynes' Principle of Maximum Entropy** subject to expected energy constraints.
- Contrastive divergence and score matching: decomposing gradient updates into positive (attractor) and negative (repulsive) thermodynamic phases.

### Chapter 3: Siamese Architectures & The Collapse Manifold
- Twin networks and shared parameter dynamics $\theta_1 = \theta_2$.
- Gradient dynamics under attraction-only losses: proving that $\nabla_\theta \mathcal{L}_{\mathrm{inv}}(\theta) = 0$ is globally minimized by constant mappings.
- The geometry of the representation sphere $\mathbb{S}^{d-1}$ and collapse equilibria.

### Chapter 4: Contrastive Learning & The Curse of Dimensionality
- InfoNCE mutual information lower bounds: $I(X; Y) \ge \log(K) - \mathcal{L}_{\mathrm{InfoNCE}}$.
- **Negative sampling complexity theorem:** In $\mathbb{R}^d$, covering number bounds dictate that uniformly repelling negative samples requires sample complexity $\mathcal{N}(\epsilon) = \mathcal{O}((1/\epsilon)^d) = \mathcal{O}(e^d)$.
- Fundamental trade-off: exponential scaling in ambient representation dimension vs. computational tractability.

### Chapter 5: Non-Contrastive Regularization: Barlow Twins & VICReg
- **Horace Barlow's redundancy reduction principle** in sensory neuroscience.
- **Barlow Twins objective:** Cross-correlation matrix of standardized representations:
  $$\mathcal{L}_{\mathrm{BT}} = \sum_{i} (1 - \mathcal{C}_{ii})^2 + \lambda \sum_{i} \sum_{j \neq i} \mathcal{C}_{ij}^2, \quad \mathcal{C} = \frac{1}{B} Z^T Z'$$
- Proof that diagonal matching forces maximum information transmission while off-diagonal minimization orthogonalizes latent basis vectors.
- **VICReg analysis:** Explicit decomposition into:
  1. *Invariance:* $s(Z, Z') = \frac{1}{B} \|Z - Z'\|_F^2$
  2. *Variance (Anti-collapse hinge):* $v(Z) = \frac{1}{d} \sum_{j=1}^d \max(0, 1 - \sqrt{\mathrm{Var}(Z_{:,j}) + \epsilon})$
  3. *Covariance (Decorrelation):* $c(Z) = \frac{1}{d} \sum_{i \neq j} \mathrm{Cov}(Z)_{ij}^2$.

### Chapter 6: World Models & Joint Embedding Predictive Architectures (JEPA)
- Yann LeCun's tripartite architecture: Encoder $s_x = f_\theta(x)$, Predictor $s_y = g_\phi(s_x, z)$, Target Encoder $\bar{s}_y = f_{\bar{\theta}}(y)$ with Exponential Moving Average (EMA) or Stop-Gradient.
- The failure of pixel-level generative world models (diffusion, autoregressive video generation) on stochastic, non-predictable task-irrelevant entropy (e.g. grass blowing in wind).
- Prediction strictly in abstract representation space $\mathcal{Z}$.
- Masked auto-encoding in latent space: spatiotemporal tubelet conditioning in I-JEPA and V-JEPA.

### Chapter 7: Statistical Invariant Geometric Regularizer (SIGReg) & The Cramér-Wold Device
- Formulation of **SIGReg**: replacing heuristic variance/covariance penalties with statistical hypothesis testing against standard isotropic Gaussians $\mathcal{N}(0, I_d)$.
- **The Cramér-Wold Theorem:** A Borel probability measure $P$ on $\mathbb{R}^d$ is uniquely determined by the family of its one-dimensional projections $P_u$ onto unit vectors $u \in \mathbb{S}^{d-1}$.
- **Epps-Pulley Test:** Statistical distance between empirical characteristic functions:
  $$\phi_n(t) = \frac{1}{n} \sum_{j=1}^n e^{i t (u^T z_j)}$$
  evaluated against the Gaussian characteristic function $\phi_0(t) = e^{-t^2 / 2}$ under weighted $L^2$ norms.
- Exact closed-form formulation of the test statistic without Monte Carlo numerical integration:
  $$T_n(u) = \frac{1}{n} \sum_{j,k} \exp\left(-\frac{\gamma}{2} (u^T z_j - u^T z_k)^2\right) - \frac{2}{\sqrt{1 + \gamma}} \sum_j \exp\left(-\frac{\gamma u^T z_j^2}{2(1+\gamma)}\right) + \frac{n}{\sqrt{1 + 2\gamma}}$$
- Establishing asymptotic properties and non-parametric bounds for preventing latent collapse in high-dimensional representation spaces.

### Chapter 8: Synthesis & Perspectives for Embodied World Models
- Integrating action conditioning $a_t \in \mathcal{A}$ into latent JEPA dynamics: $s_{t+1} = g_\phi(s_t, a_t)$.
- $\mathrm{SE}(3)$-equivariance in robotics and spatial perception.
- Open challenges: partial observability, non-Markovian latent state spaces, and sample-efficient exploration.

---

## 🛠️ Repository Organization

```text
├── TFG_PDF_LATEX_PROJECT/
│   ├── main.pdf                    # Complete compiled 100+ page monograph
│   ├── main.tex                    # LaTeX root file
│   ├── capitulo1.tex - capitulo8.tex# Chapter source files
│   ├── apendice_al.tex             # Appendix on Linear Algebra & Spectral Theory
│   ├── apendice_dl.tex             # Appendix on Deep Learning Foundations
│   ├── referencias.bib             # 100+ bibliography entries
│   └── *.pdf / *.png               # Mathematical figures (Cramér-Wold, epsilon-nets, LeJEPA)
│
└── presentacion_proyecto/
    ├── presentacion.pdf            # 64-slide Beamer defense presentation
    ├── presentacion.tex            # Beamer LaTeX source
    └── guion.md                    # Narrative defense script for tribunal presentation
```

---

## 📖 Citation

If you consult or build upon the mathematical formulations in this work:

```bibtex
@mastersthesis{rivasvega2024ebm_jepa,
  author       = {Pedro Rivas Vega},
  title        = {{Fundamentos Matemáticos de los Energy-Based Models y las Joint Embedding Architectures: Del Representation Collapse a los World Models}},
  school       = {Universidad de Sevilla, Departamento de Matemática Aplicada II},
  year         = {2024},
  month        = {July},
  note         = {Supervised by Prof. Eduardo Sánchez Karhunen. Highest Honors (Matrícula de Honor candidate)}
}
```

---

## 👤 Author

**Pedro Rivas Vega**  
*M2 MVA, École Normale Supérieure Paris-Saclay*  
*B.S. in Mathematics, Universidad de Sevilla*  
- GitHub: [@rivas-vega](https://github.com/rivas-vega)
- Email: [pedrorivasvega@gmail.com](mailto:pedrorivasvega@gmail.com)
