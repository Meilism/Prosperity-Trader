# Prosperity-Trader
This is the repo to test and develop trading algorithm for the IMC prosperity trading challenge.

## Setup
Create a conda environment `trader` and activate the environment with the following command:
```bash
conda create -n trader python=3.12
conda activate trader
```
Install the required packages:
```bash
conda install pandas numpy jsonpickle matplotlib ipykernel
```
Note that `matplotlib` is for visualization, `ipykernel` is required to run the jupyter notebook in the conda environment, neither of them is required for the trading simulation.

**Example**
An example [trader.py](trader.py) is provided in the repo.
The file is an empty skeleton and does not have any trading strategy coded up.

:warning: **People could make their own copy of the file as `trader_<name>.py` and implement their own trading strategy.**

## Use the Visualizer and Back-tester
The skeleton file contains the necessary modification to use the [IMC Prosperity 2 Visualizer](https://jmerle.github.io/imc-prosperity-2-visualizer/).

**Visualizer**
To use the visualizer, follow the steps below:
1. Modify the `trader_<name>.py` file to implement your own trading strategy.
2. Upload the `trader_<name>.py` to the Prosperity server.
3. After the server finishes the simulation, download the log file from the server and upload it to the visualizer.
4. Check out the performance of your trading strategy there!

**Back-tester**
To use the [backtester](https://github.com/jmerle/imc-prosperity-2-backtester), follow the online instructions to install the package and run in the terminal.
Note that the code only does the order matching on the existing orders in `order_depth`, but in the real prosperity server, the order matching can also happen with hidden orders with virtual bots.

The backtester package is compiled to load data from some `.csv` files stored under `[installation folder for the package]/resources/` which are extracted from prosperity log files.
Since it also provides support for custom data source, I also create a `shared_data` folder to store some custom data files.
More specifically, for round 1, I have created a `shared_data/round1` folder to store the offical data files for round 1 and `shared_data/round6` for a subset of the data files for round 1.

The naming here is for convenience only - there is no round 6 in the prosperity challenge, and the naming is just to make sure that if you specify the round number in the backtester, it will load the data from the correct folder.

To use the backtester, follow the steps below:
1. Modify the `trader_<name>.py` file to implement your own trading strategy.
2. Install the backtester package
3. Run the backtester with the following command to get the results for a specific round:
```bash
prosperity2bt <path-to-your-file> <round-number> --data shared_data
```
Note that by specifying the `--data shared_data` flag, the backtester will load the data from the `shared_data` folder and you can actually test on a smaller subset of the data in round 1 by specifying the round number to be 6.

## Use `traderData` to store data between iterations
The algorithm needs some data to make trading decisions, which should be stored in the `traderData` object to make sure it is persistent between iterations.

All user data is stored in `traderData` and needs to be serialized to `string` object before returning to the simulator.
```python
import jsonpickle
traderData = jsonpickle.encode(traderData)
```
To use the `traderData` in the next iteration, deserialize the `string` object back to `traderData`.
```python
traderData = jsonpickle.decode(traderData)
```