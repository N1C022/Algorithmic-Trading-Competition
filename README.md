# Imperial AlgoSoc Trading Competition

Adaptive market-making bot written under timed conditions for the Imperial College AlgoSoc Trading Competition. **Placed Top 5 out of ~100 teams.**

## Overview

A stateful trading algorithm that reacts to short-term price signals via exponential moving averages and adapts its order placement to current market conditions. Built in a Jupyter Notebook in a fixed time window, with a custom backtester.

## Strategy

The bot operates as a passive market maker with three adaptive components:

**1. Signal generation via EMAs.**
Computes short-window and longer-window exponential moving averages of mid-price. The relationship between fast and slow EMAs forms the basis of short-term directional signal.

**2. Spread capture via previous-tick quoting.**
Places buy and sell orders at the best price from the previous tick. The backtester classifies this as "perfect market making" — the bot captures the spread when its quotes are matched on the next tick.

**3. Confidence-scaled and inventory-aware sizing.**
Order size scales up when signal confidence is high (EMAs aligned, recent volatility low) and scales down when uncertain. Inventory accumulation triggers asymmetric skewing — when long, the bot tightens its ask and widens its bid to encourage offloading.

**4. Random perturbation for local-minimum escape.**
Occasional random adjustment to parameters to avoid getting stuck in suboptimal regimes when the strategy becomes statically tuned to one market state.

## Files

| File | Purpose |
|---|---|
| `submission_notebook.ipynb` | Main strategy implementation and competition submission |
| `backtest.py` | Custom backtester for evaluating strategy variants |
| `extra_classes.py` | Helper classes for order management and state tracking |
| `train.csv` / `test.csv` | Competition data |

## Running

1. Clone the repository
2. Open `submission_notebook.ipynb` in Jupyter
3. Run all cells

To switch between training and test data, edit the `data` variable in `backtest.py`.

## What I'd do differently

- **Better signal**: EMAs are a basic starting point. Pairing them with a volatility regime detector would let the bot widen spreads in turbulent periods and tighten them in calm ones.
- **Smarter inventory penalty**: current skewing is rule-based; an explicit cost-of-inventory model in the optimisation would be cleaner.
- **More rigorous parameter selection**: random perturbation worked as a heuristic but is not a substitute for proper hyperparameter search.

These weren't possible under the time constraints of the competition.

## Notes

Submitted as part of the AlgoSoc Course Trading Competition. The Top 5 placement was determined by P&L on the held-out test data set, with ~100 teams participating.
