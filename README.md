
# MARL-Eco-5G: Multi-Agent Deep RL for Energy-Cost Co-Optimization in 5G/6G Networks

Companion code for: *Multi-Agent Deep Reinforcement Learning for Energy-Cost
Co-Optimization in 5G/6G Networks: A Techno-Economic Simulation Framework*
(Energy Engineering, Manuscript ID 91065).

**DOI (Zenodo):** [TO COMPLETE — e.g. 10.5281/zenodo.XXXXXXX]

## Environment
- Python >= 3.10, PyTorch >= 2.0 (see `requirements.txt`)
- Dataset (input, not redistributed): ITU 5G Energy Consumption Dataset,
  https://www.kaggle.com/datasets/nadiatriki/5g-energy-consumption-dataset
  (download and place `5G_energy_consumption_dataset.csv` in `data/`; the
  environment falls back to synthetic diurnal profiles if absent, so all
  scripts run out of the box).

## Reproduction guide (paper table/figure -> command)

| Paper item | Command |
|---|---|
| Table 2 (simulation parameters) | `config/sim_params.yaml` |
| Fig. 2(a), Table 3 rows (learning curves, KPIs, 10 seeds) | `python scripts/train.py --algo mappo --episodes 1000 --n-agents 16` |
| Table 6 (ablation a-e) | `python scripts/train.py --algo mappo --ablation tou` (idem: `switch`, `batt`, `std`) |
| Scalability N=50/100 | `python scripts/train.py --algo mappo --n-agents 50` (idem: `100`) |
| Table 5 (deterministic cash flows) | `python scripts/monte_carlo_finance.py --deterministic` |
| Table 4 (Monte-Carlo, N=10,000) | `python scripts/monte_carlo_finance.py --runs 10000` |
| Table A2 (discount-rate sensitivity) | `python scripts/monte_carlo_finance.py --rates 0.04 0.06 0.08 0.10 0.12` |
| Maintenance sensitivity (-20/-35/-50 %) | `python scripts/monte_carlo_finance.py --maint-scans` |
| Fig. 2(b)/3(a)/3(b) | produced by `train.py` logs in `results/` |

All randomness is fixed by `config/seeds.txt` (10 seeds, one per line).

## Layout
- `src/environment.py` — EARTH-based power model (Eq. 4-5), battery fade
  (Section 3.4: cycle + calendar, replacement at 20 %), ToU tariffs, QoS model.
- `src/rewards.py` — R_eco (Eq. 8) and the five ablation configurations.
- `src/mappo.py` — **on-policy MAPPO (revised Algorithm 1)**: rollout collection
  with stored old-policy log-probabilities, K=4 PPO epochs, buffer discarded
  after each update. No experience replay.
- `src/baselines.py` — Always-On, Round-Robin Sleep, MADQN, MAPPO-Standard.
- `scripts/train.py` — multi-seed training and KPI export (Table 3).
- `scripts/monte_carlo_finance.py` — **incremental cash-flow analysis
  (revised Algorithm 2)**: explicit upfront investment I0, NPV, ROI = NPV/I0,
  IRR (initial negative flow), payback; Tables 4, 5, A2.
- `config/` — simulation, financial, and seed configuration.

## Citation
    Lazrek H., Sbai El Hassani L. O., Zouiten M. (2026). Multi-Agent Deep
    Reinforcement Learning for Energy-Cost Co-Optimization in 5G/6G Networks.
    Energy Engineering. DOI: [TO COMPLETE]

## License
MIT (see LICENSE).