# Claude Code — working rules for this repo

Read `README.md` and the existing `src/` modules before writing code. Match the
established interfaces, naming, and docstring style.

## Non-negotiable rules
- **TESTNET ONLY.** No real-money account, no real orders. ccxt sandbox mode is
  forced on in every client — keep it that way.
- **Keys from env only** — never hard-code or commit credentials.
- **Do not change the event-driven architecture** or break the frozen interfaces
  in `src/core/events.py` and `src/strategy/base.py`.
- **Keep mock mode working at all times** (`python main.py`) — it's how the team
  tests offline.
- After completing a step, summarize what changed and what to verify, then wait.

## Module ownership
Data/Strategies/Dashboard: Cheng, Gilbert · Regime: ShiYi · Portfolio/Risk:
Gilbert, Grace · Execution/OMS: sookoon.

## Step-by-step plan — DO IN ORDER, stop and report after each
1. **Confirm the data feed** ← current step. `python scripts/check_data.py`
   should print live BTC/ETH books + `OK - data feed is working.` (no key needed).
2. **Observe-only validation.** Live testnet data → regime + strategy, execution
   disabled. Log regimes/signals ~30 min; place no orders.
3. **First live order.** Enable OMS with `CcxtBroker`; one small manual market
   order; confirm fill + Portfolio + testnet balance update.
4. **Order lifecycle.** Limit orders, 5s TTL cancel, kill orders, partial fills,
   inventory balancing. (TODOs in `src/part3_execution/oms.py`.)
5. **Risk hardening.** Historical VaR (10-min), stop-loss/take-profit, drawdown
   alerts + halt. (TODOs in `src/part5_risk/risk_manager.py`.)
6. **Regime upgrade.** GaussianHMM (`hmmlearn`) + entropy feature. (`src/part6_regime/`.)
7. **Dashboard.** Wire Streamlit to live state. (`src/part7_dashboard/app.py`.)
8. **Backtester.**
9. **Class diagram + 5-min pitch.**

See `README.md` for architecture, layout, and current status.
