# `strategies/`

Trading books. A strategy is any object with `signal(bars) -> np.ndarray`
returning the target position in `[-1, 1]` per bar (see `base.py`). Each book is
`@register`-ed under a short `name` and resolved through `REGISTRY` by the
backtest engine, `validate_books.py`, and the live runner. Full research
narrative per book: `docs/STRATEGY_PLAYBOOK.md`.

| File | `name` | Description |
|------|--------|-------------|
| `base.py` | — | Strategy `Protocol` + `register`/`REGISTRY`. Signals must use data up to the current bar only (engine shifts execution by one). |
| `__init__.py` | — | Imports every module to populate `REGISTRY`. |
| `vwap_trend.py` | `vwap_trend` | Session-VWAP stretch continuation. |
| `session_vol.py` | `london_orb` | London open range breakout. |
| `sweep_fade.py` | `sweep_fade` | Prior-day high/low sweep fade ("liquidity sweep" reversal). |
| `meanrev_pairs.py` | `ratio_mr` | Mean reversion on a synthetic spread; RANGE-regime gated. Includes `spread_bars()` helper. |
| `regime_switch.py` | `regime_switch` | Regime-conditional: ride trend / fade range extremes. Range-fade leg off by default. |
| `orderflow_imbalance.py` | `ofi` | Order-flow imbalance momentum (needs real L2 depth). |
| `fast_meanrev.py` | `fast_mr` | Rolling-mean z-score fade (state machine). |
| `strategy_template.py` | — | Copy-this-file template for a new strategy. **Not registered.** |

Per-book weights live in `config/portfolio.yaml` (set your own).
