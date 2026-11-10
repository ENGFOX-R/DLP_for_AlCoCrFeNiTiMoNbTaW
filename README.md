# Deep Learning Potential for Ten- and Six-Component FCC Al-Co-Cr-Fe-Ni-Ti Concentrated Alloys with Mo-Nb-Ta-W Additions

This repository provides the trained Deep Learning Potential (DLP) model, training configurations, dataset structure, LAMMPS molecular dynamics simulation scripts, and initial atomic configurations supporting the paper:

> **"Deep Learning Potential Study of MoNbTaW Effects in Ten- and Six-Component FCC Al-Co-Cr-Fe-Ni-Ti Concentrated Alloys"**

---

## 📂 Repository Structure

```text
.
├── DLP/
│   ├── graph-compress01.pb       # Trained & compressed DeePMD-kit model (Git LFS / GitHub Release)
│   ├── graph01.json              # DeePMD-kit training configuration file
│   ├── graph01-compress.json     # DeePMD-kit model compression configuration file
│   └── all_npy/                  # NumPy-formatted training dataset
│       └── initial/
│           ├── Al7Co7Cr2Fe2Mo2Nb2Ni2Ta2Ti2W4_fcc_bulk-T300-P0/  # Representative training & validation arrays
│           └── ... (108 composition systems)
├── tension/
│   ├── M1/                       # Ten-component concentrated alloy (M1)
│   │   ├── tension_simulation.in # LAMMPS uniaxial tensile simulation script (300 K)
│   │   └── out-fcc-Al04Co33Cr22Fe15Mo01Nb01Ni25Ta01Ti02W01_out_paper_expRATIO_supC111122_10648atoms.data
│   └── M2/                       # Six-component reference alloy (M2)
│       ├── tension_simulation.in # LAMMPS uniaxial tensile simulation script (300 K)
│       └── out-fcc-Al04Co34Cr23Fe15Mo00Nb00Ni25Ta00Ti02W00_out_paper_expRAT_del_Rele_supC111122_10648atoms.data
└── README.md
```

---

## 🔬 Description of Materials

### 1. Trained DLP Model (`DLP/graph-compress01.pb`)
- Trained with **DeePMD-kit v3** using a smooth two-body embedding descriptor (`se_a`).
- **Embedding net**: `{25, 50, 100}`
- **Fitting net**: `{160, 160, 160}`
- **Cutoff radius ($r_c$)**: 6.0 Å (smoothing from 5.8 Å).
- Elements supported (10 elements): **Al, Co, Cr, Fe, Ni, Ti, Mo, Nb, Ta, W**.
- **Model Download**: Due to GitHub file size limits (>100 MB), the model file (425.7 MB) is tracked via **Git LFS** and is also hosted directly on the [GitHub Releases](https://github.com/ENGFOX-R/DLP_for_AlCoCrFeNiTiMoNbTaW/releases/tag/v1.0.0) page:
  - 📥 [Direct Download `graph-compress01.pb`](https://github.com/ENGFOX-R/DLP_for_AlCoCrFeNiTiMoNbTaW/releases/download/v1.0.0/graph-compress01.pb)

### 2. DeePMD-kit Training Configuration (`DLP/graph01.json`, `DLP/graph01-compress.json`)
- Contains full training hyperparameter definitions:
  - Two-stage learning rate schedule: starting at $1.0 \times 10^{-3}$, decaying exponentially to $3.51 \times 10^{-8}$ over $1.0 \times 10^6$ training steps.
  - Dynamically weighted multi-task loss function balancing energy, atomic forces, and virial tensors.
  - Batch size, descriptor parameters, and compression settings.

### 3. Training Dataset (`DLP/all_npy/`)
- Contains NumPy-formatted training data associated with the Universal Materials Accelerator (UMA) foundation-model-labeled atomic configurations.
- Demonstrates the directory hierarchy for all 108 composition systems with a fully populated representative system (`Al7Co7Cr2Fe2Mo2Nb2Ni2Ta2Ti2W4_fcc_bulk-T300-P0`), including `box.raw`, `coord.raw`, `energy.raw`, `force.raw`, `virial.raw`, `type.raw`, and validation sets.
- Full 54,000-frame database archive can be reproduced using the provided configurations or requested upon need.

### 4. LAMMPS Tensile Simulation Scripts (`tension/M1/`, `tension/M2/`)
- Input scripts (`tension_simulation.in`) for uniaxial tensile deformation along the z-direction at 300 K under constant engineering strain rate ($1.0 \times 10^8\text{ s}^{-1}$ or $1.0 \times 10^9\text{ s}^{-1}$).
- Utilizes the `pair_style deepmd` plugin interface.
- Implements NPT relaxation followed by non-equilibrium tensile deformation with periodic boundary conditions.

### 5. Initial Atomic Configurations (`.data` files)
- **M1 alloy** (10 components): 10,648-atom FCC supercell structure matching experimental composition ratios ($Al_{0.4}Co_{3.3}Cr_{2.2}Fe_{1.5}Mo_{0.1}Nb_{0.1}Ni_{2.5}Ta_{0.1}Ti_{0.2}W_{0.1}$).
- **M2 alloy** (6 components): 10,648-atom FCC supercell reference structure ($Al_{0.4}Co_{3.4}Cr_{2.3}Fe_{1.5}Ni_{2.5}Ti_{0.2}$).
- Both structures include precise initial simulation cell parameters, atomic coordinates, and species assignments.

---

## 🚀 Quick Start & Usage

### Running LAMMPS Tensile Simulations with DeePMD-kit

1. Ensure **LAMMPS** is compiled with the **DeePMD-kit C++ plugin** (`libdeepmd_lmp.so`).
2. Download `graph-compress01.pb` into the `DLP/` directory:
   ```bash
   git lfs pull
   # or manually download into DLP/:
   # wget https://github.com/ENGFOX-R/DLP_for_AlCoCrFeNiTiMoNbTaW/releases/download/v1.0.0/graph-compress01.pb -P DLP/
   ```
3. Run the tensile simulation:
   ```bash
   cd tension/M1
   lmp -in tension_simulation.in
   ```

---

## 📌 Citation & Data Availability

If you use this potential, training data, or simulation setups in your research, please cite:

```bibtex
@article{ju2026deep,
  title={Deep Learning Potential Study of MoNbTaW Effects in Ten- and Six-Component FCC Al-Co-Cr-Fe-Ni-Ti Concentrated Alloys},
  author={Ju, Shin-Pon and co-authors},
  journal={Computational Materials Science},
  year={2026}
}
```

- **GitHub Repository**: [https://github.com/ENGFOX-R/DLP_for_AlCoCrFeNiTiMoNbTaW](https://github.com/ENGFOX-R/DLP_for_AlCoCrFeNiTiMoNbTaW)
- **Zenodo DOI**: *(Minted upon repository release; see instructions below)*
