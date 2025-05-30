README.md
# Pendulum‑Kalman Sandbox 🌀

A **minimal, opinionated playground** for state‑estimation on a single‑link, friction‑free pendulum with Gaussian sensor noise. Swap in any Kalman‑filter flavour (classic, EKF, UKF, etc.) and watch the estimates dance against ground truth in real‑time plots and animations.

> **Why?** Because textbooks stop at equations. This repo lets you *see* them breathe.

---

## 📂 Repository layout

| Path | What lives here |
|------|-----------------|
| `kfmodels.py` | Abstract `KalmanFilterBase` (+ helpers) |
| `pendulummodel.py` | Non‑linear plant: θ̈ + (g/ℓ) sin θ = 0 |
| `pendulum.py` | **Simulation engine** – advances the plant, calls the filter, plots/animates |
| `pendulum_filter.py` | Ready‑to‑run demo: small‑angle linear KF + default settings |
| `pendulum_sims.py` | Compare non‑linear vs linearised dynamics (no filtering) |

Add‑your‑own filters by subclassing `KalmanFilterBase` and pointing `pendulum.py` at it.

---

## 🚀 Quick start

```bash
# 1. Clone & enter
git clone https://github.com/<you>/pendulum‑kalman.git && cd pendulum‑kalman

# 2. Install deps (Python ≥3.9)
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt  # numpy, scipy, matplotlib, tqdm

# 3. Run the linear‑KF demo with pretty plots & an animation pop‑up
python pendulum_filter.py
```

You should see three subplots (θ, ω, measurement) and a live animation of the blue “truth” vs red “estimate”. Close the window to finish.

---

## 🔄 Swapping filters

1. Create `my_kf.py`:

```python
from kfmodels import KalmanFilterBase

class MyUnscentedKF(KalmanFilterBase):
    def _predict(self, u):
        ...
    def _update(self, z):
        ...
```

2. Point the engine at it:

```python
from pendulum import run_sim
from my_kf import MyUnscentedKF
run_sim(MyUnscentedKF)
```

All plotting / animation comes for free.

---

## ⚙️ Tweakables (`sim_options`)

| Key | Default | Meaning |
|-----|---------|---------|
| `dt` | `0.01` s | Integration & update step |
| `t_end` | `10` s | Total simulated time |
| `meas_std` | `0.05` rad | Std‑dev of angle sensor |
| `torque_std` | `0.1` N·m | Process (model) noise |
| `animate` | `True` | Generate Matplotlib animation |
| `seed` | `42` | RNG seed for repeatability |

---

## 📝 Requirements

* Python 3.9+
* numpy ≥ 1.26
* scipy ≥ 1.11
* matplotlib + ffmpeg (for MP4 export)

Everything is pure‑Python; no Cython/pybind pain.

---

## 📈 Screenshots

*(insert GIF of animation + plots here)*

---

## 🛣️ Roadmap / TODO

- [ ] EKF implementation with full Jacobians
- [ ] Unscented KF (UKF) drop‑in
- [ ] Add option for control torques (PID swing‑up demo)
- [ ] CI workflow for lint + minimal test run

---

## 📜 License

MIT – do **anything** you want, just leave the credit.

## 🙏 Acknowledgements

Built with ☕, numpy, and the timeless beauty of classical mechanics.

