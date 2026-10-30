# Model-Agnostic Machine Learning MPPT for Photovoltaic Systems

**Development and validation of a model-agnostic ML framework for PV performance under complex weather conditions**

Abdelhak Samir Douli, Abd Essalam Badoud, Belkacem Ould Bouamama, Mahdi Boukerdja
*LAS, Université Sétif 1 (Algeria) · CRIStAL UMR 9189, Université de Lille (France)*

📄 Accepted for publication · Journal and DOI: _to be added_ <!-- TODO: add journal + DOI -->

---

![Overview of the model-agnostic ML-MPPT](figures/overview.jpg)

## Idea

A neural network that predicts the maximum power point voltage of **any** PV module, without retraining.
The network gets the module's datasheet parameters as inputs, together with the irradiance and temperature measurements. The same pre-trained weights can then be deployed on a new panel by entering its parameters.

## Method

* **Dataset:** 2,187 parameterized PV models (Ns, Rs, Rsh, n, I₀, α_Isc, β_Voc), irradiance from 200 to 1600 W/m², temperature from 0 to 60 °C. That gives more than **10 million samples**, each labelled with the exact V_MPP from **Brent's method**.
* **Model:** feed-forward network with a bottleneck (9 inputs → 16 → 9 → 1), small enough for real-time inference.
* **Control:** the predicted V_MPP sets the boost converter duty cycle through a proportional law.
* **Benchmark:** P&O, incremental conductance (INC) and fuzzy logic control (FLC) on **four commercial modules**: Aavid Solar ASMS-165P, A2Peak POWER ON P220-6x10, Aavid Thermalloy ASMP-175M and Ablytek 5MN6C175-A0. Each was tested under standard conditions, irradiance steps, temperature steps and combined variations, in MATLAB/Simulink.

## Results (summary over the four modules)

| Method | Avg. tracking efficiency | Power oscillations | Needs retuning for a new module? |
|---|---|---|---|
| **ML (this work)** | **> 99.9 %** | **± 0.03 – 0.06 W** | **No**, only module parameters are entered |
| Fuzzy logic | 98.8 – 99.1 % | ± 1.5 – 2.5 W | Yes, the rule base must be redesigned |
| P&O | 95.5 – 97.1 % | ± 1.4 – 3.2 W | Step size must be retuned |
| INC | 91.4 – 99.8 % | ± 0.5 – 1.8 W | Works, but performance is inconsistent |

The ML controller settles in about **70 ms**.

![Voltage tracking under variable conditions](figures/variable_conditions_voltage.jpg)
*PV voltage under combined variations: the ML controller (green) moves directly to each new MPP, without the oscillations of P&O or the drift of INC.*

## Follow-up work

* [bg-ml-mppt-pv](https://github.com/abdelhaksamirdouli/bg-ml-mppt-pv) extends this idea with a **bond graph** model of the module, a single-module ablation and a sensitivity study (accepted in *Energies*).
* [pemfc-ml-mppt-hil](https://github.com/abdelhaksamirdouli/pemfc-ml-mppt-hil) applies the same data-generation strategy to **PEM fuel cells**, with hardware-in-the-loop validation.

## Tools

MATLAB/Simulink · Python · PyTorch · ONNX · SciPy

## Code

The simulation models and training code are kept in a private repository during the PhD. They are **available on request**: douli.a@univ-setif.dz.

## Funding

This work was supported by the PHC Tassili project No. 51327SE, *Integrated Design of Resilient Systems for Green Hydrogen Production*.
