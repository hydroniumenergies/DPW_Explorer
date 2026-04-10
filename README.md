# Differential Pressure Window Explorer v3

**Interactive Supplementary Figure S1 — Physically Validated Model** for:

> Rahman, A., Singh, B. & Zhou, C. (2026). *Differential Pressure Engineering as a Primary Strategy to Mitigate Carbonate Formation in CO₂ Electrolysers*. Perspectives in Energy & Electrochemistry. DOI: 10.1039/XXXX

**→ [Open live tool](https://YOUR-USERNAME.github.io/dpw-explorer/)**

---

## Version history

| Version | Key change |
|---------|-----------|
| v1.0 | Simplified algebraic Δp_min, fixed k_m |
| v2.0 | Extended to 1000 mA cm⁻², FE/EE/utilisation outputs, three catalysts |
| **v3.0** | **GHK–Darcy exact analytical solution · CO₂/OH⁻ speciation mechanism · Mass transport overpotential · k_m as measurable input slider** |

---

## Physics model — v3

### Transport: GHK–Darcy exact solution (Eq. A1–A2)

The carbonate flux across the AEM is computed from the exact analytical solution to the 1D steady-state Nernst–Planck–Darcy equation:

```
J_net = (D/δ) · Pe/(exp(Pe)−1) · (c₀·exp(Pe) − c_L)
```

where the combined Peclet number Pe = Pe_electric + Pe_hydraulic accounts for both electromigration and hydraulic convection. Numerically stable for all Pe (Taylor expansion near Pe=0, asymptotic limits at |Pe|>50).

Δp_min is solved numerically (52-step bisection) from J_net = 0.

### Carbonate suppression: CO₂/OH⁻ competition at cathode interface (Eq. A3)

The dominant suppression mechanism is thermodynamic, not convective: elevated p_CO₂ increases [CO₂(aq)] at the cathode surface, which competes with the electrochemically-produced OH⁻ for the carbonate-forming pathway. The interfacial carbonate concentration is:

```
c₀(j, Δp) = c_carb,bulk · 1 / (1 + 2.5 · [CO₂(aq)] / [OH⁻]_local)
```

where [OH⁻]_local includes both the diffusion-layer OH⁻ from electroreduction and the bulk value. Competition coefficient 2.5 calibrated to pH 7–9 operating regime.

### Performance outputs (Eq. B1–B5)

- **FE(j, Δp)**: Intrinsic FE₀(j) from literature-parameterised catalyst tables, degraded by carbonate penalty: FE = FE₀ · (1 − α · X_carb)
- **V_cell**: Butler–Volmer cathode kinetics (b, j₀ per catalyst) + Tafel OER anode + GDE gas-phase mass transport overpotential (Fick diffusion through GDL+MPL) + carbonate-dependent membrane resistance
- **EE**: FE · (ΔG°/n_eF) / V_cell
- **η_util**: Stoichiometric CO₂ consumption + carbonate regeneration bonus

### Key parameter: membrane hydraulic permeability k_m

The k_m slider covers 10⁻¹⁹ to 10⁻¹⁶ m² (log scale) — the full range of commercial and engineered AEMs. **Measure k_m for your specific membrane** (dead-end filtration cell, ~30 min) before using Δp_min as a quantitative design target.

| AEM type | Typical k_m (m²) |
|----------|-----------------|
| Dense polyphenylene (commercial) | 10⁻¹⁹ – 10⁻¹⁸ |
| SEBS-based AEM | 5×10⁻¹⁹ – 5×10⁻¹⁸ |
| Composite / reinforced | 10⁻¹⁸ – 10⁻¹⁷ |
| ePTFE-supported thin-film | 10⁻¹⁷ – 10⁻¹⁶ |

---

## Accuracy statement

| Output | Accuracy | Condition |
|--------|----------|-----------|
| Δp_min | Quantitative | If k_m measured for your membrane |
| DPW boundaries | Quantitative | Same |
| FE(j) curves | Semi-quantitative (±20%) | X_carb model fitted to pH 8 regime |
| EE(j) curves | Semi-quantitative (±25%) | V_cell model: BV + Tafel + Fick MT |
| η_util | Qualitative trends only | Regeneration bonus is estimated |
| η_mt | Quantitative at 1 bar; semi at high Δp | GDE Fick model, gas-phase only |

---

## Catalyst parameters

| Catalyst | FE₀ source | b (mV/dec) | j₀ (mA cm⁻²) | α |
|----------|-----------|-----------|-------------|---|
| Cu (C₂+) | Dinh et al. *Science* 2018; Luc et al. *JACS* 2019 | 130 | 10⁻⁴ | 0.65 |
| Ag (CO) | Gabardo et al. *EES* 2019; Verma et al. *ACS EL* 2018 | 110 | 8×10⁻³ | 0.40 |
| Bi/Sn (formate) | Zheng et al. *Nat. Catal.* 2019; Zhang et al. *Angew. Chem.* 2020 | 120 | 2×10⁻³ | 0.35 |

---

## How to cite

```bibtex
@article{rahman2026differential,
  title   = {Differential Pressure Engineering as a Primary Strategy to
             Mitigate Carbonate Formation in {CO}$_2$ Electrolysers},
  author  = {Rahman, A. and Singh, B. and Zhou, C.},
  journal = {Perspectives in Energy \& Electrochemistry},
  year    = {2026},
  doi     = {10.1039/XXXX},
  note    = {Interactive model v3: \url{https://YOUR-USERNAME.github.io/dpw-explorer/}}
}
```

## Deploy to GitHub Pages

1. Fork/clone this repo
2. Settings → Pages → Source: main branch, / root → Save
3. Live at `https://YOUR-USERNAME.github.io/dpw-explorer/`

See `DEPLOY.txt` for full step-by-step instructions.

## Licence

Code: MIT. Scientific content: © The Authors, 2026.
