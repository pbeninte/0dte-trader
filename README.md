# 0dte-trader
Trade 0DTE options algorithmically using Interactive Brokers (IBKR) API.

WIP, initial target is Bull Put/Bear Call and Iron Condor. Iron Butterfly may be supported if I have time.

## Installation
Python 3.10 is assumed. 
1. Create a virtual environment and then `pip install ibapi-10.20.1-py3-none-any.whl` to install the IBKR API.
2. Run the code by executing `python -m trading.Program` with the command line options or environment variables (WIP) as specified below. e.g. to trade a Bull Put, run `python -m trading.Program -p 4002 -q 1 -t SPX -m 1 -s 0.05 -l 0.03 -x 3.0 -e 1`

## Command Line Arguments / Environment Variables

IBKR Gateway or TWS is assumed to be running in local machine or the same Kubernetes Pod (K8s setup config and instructions to automatically trade in the cloud will be available for a reasonable fee, TBD).

- `-p` (`PORT`): The TCP port to the IBKR Gateway or TWS.
- `-q` (`QUANTITY`): The amount of stock or futures option combos to trade
- `-d` (`DRY_RUN`): Dry run. If set (by passing "True"), all logic will execute as normal but will not send any orders to IBKR.
- `-m` (`MODE`): Mode: 1 for Bull Put, 2 for Iron Condor(WIP)
- `-s` (`SHORT_LEG_DELTA`): Delta of the short leg. Should be a float in range of [0,1].
- `-l` (`LONG_LEG_DELTA`): Delta of the long leg. Should be a float in range of [0,1].
- `-x` (`STOP_LOSS_PERCENTAGE`): Percentage of stop loss as the premium received. e.g. 3.0 for setting stop loss at 300 percent of premium received.
- `-e` (`DAY_TO_EXPIRY`): Day to expiry for the target option contract(s).
- `-t` (`TICKER`): Ticker to trade. Can be US stocks or indexes with options only.
- `-ai` (`AUTO_RETRY_INTERVAL`): Due to the large bid-ask spread, option spread orders may have difficulty filling at mid prices. If set and is larger than zero, order will be resubmitted by the interval specified. In each interal, order price will be decremented by the amount specified by param `-ap`. If not set or set at 0, orders will wait indefinitely until completely filled.
- `-ap` (`AUTO_RETRY_PRICE_DECREMENT`): Decrements price towards 0 by the value specified each time the order is resubmitted.
    

