# SF-Signal

A template project for developing, researching, and backtesting trading signals.

## Project Structure

```
sf-signal/
├── app/
│   ├── signal_research.py        # Explore and develop signal ideas (do not edit)
│   ├── research.py               # Analyze signal characteristics (do not edit)
│   ├── production.py             # View backtest performance (do not edit)
│   └── run_backtest.py           # Run the backtest (edit config only)
├── src/
│   └── create_signal.py          # Your signal implementation (edit this)
├── data/
│   ├── signal.parquet            # Output: Your signal
│   └── weights.parquet           # Output: Backtest weights
└── README.md
```

## Workflow

### 1. **Explore Signal Ideas** (`signal_research.py`)
   - Open interactive notebook to explore data and test signal logic
   - Load historical market data
   - Test different signal calculations
   - Visualize signal properties

   ```bash
   marimo edit app/signal_research.py
   ```

### 2. **Implement Signal** (`create_signal.py`)
   - Copy your signal logic from signal_research.py into `create_signal.py`
   - Customize date ranges, data columns, and calculation logic
   - Save signal to `data/signal.parquet`

   ```bash
   python src/create_signal.py
   ```

### 3. **Research Signal** (`research.py`)
   - Analyze quantile portfolios based on your signal
   - Explore signal characteristics before backtesting
   - Adjust quantile levels interactively
   - View performance metrics by quantile

   ```bash
   marimo edit app/research.py
   ```

### 4. **Run Backtest** (`run_backtest.py`)
   - Run MVO-based backtest on your signal
   - Generates optimal portfolio weights
   - Saves results to `data/weights.parquet`

   ```bash
   python app/run_backtest.py
   ```

### 5. **Analyze Performance** (`production.py`)
   - View final portfolio performance
   - Analyze backtest returns, drawdowns, and metrics
   - Inspect portfolio weights and allocations

   ```bash
   marimo edit app/production.py
   ```

## Data Files

All data files are stored in the `data/` directory:

- **`data/signal.parquet`**: Output from `create_signal.py`
  - Columns: `date`, `barrid`, `alpha` (your signal)
  - Format: Parquet (AlphaSchema)

- **`data/weights.parquet`**: Output from backtest
  - Contains: Portfolio weights and performance data
  - Format: Parquet

## Quick Start

```bash
# 1. Explore signal ideas
marimo edit app/signal_research.py

# 2. Implement your signal
# Edit src/create_signal.py with your logic
python src/create_signal.py

# 3. Research signal characteristics
marimo edit app/research.py

# 4. Run backtest
python app/run_backtest.py

# 5. View performance
marimo edit app/production.py
```

## Modifying Your Signal

Edit `src/create_signal.py` to customize:

- **Date range**: Change `start_date` and `end_date`
- **Data columns**: Modify the columns loaded from CRSP
- **Signal logic**: Replace or extend the signal calculation
- **Output path**: Change the default save location

Example:
```python
def create_signal(output_path: str = "data/signal.parquet"):
    # Your custom logic here
    signal_df = df.select([
        pl.col("date"),
        pl.col("barrid"),
        pl.col("your_signal_calculation").alias("alpha")
    ])
    AlphaSchema.write_parquet(signal_df, output_path)
```

## Template Files (Do Not Edit)

The following files in the `app/` folder are templates and should not be modified:
- `app/signal_research.py` - Use for signal exploration and development
- `app/research.py` - Automatically loads and analyzes your signal
- `app/production.py` - Automatically loads and displays backtest results
- `app/run_backtest.py` - Runs the backtest (only edit configuration if needed)

**All signal customization happens in `src/create_signal.py`.**

## Configuration

Update `run_backtest.py` if needed:
- `byu_email`: Your BYU email for job submission
- `gamma`: Transaction costs or risk aversion parameter
- `constraints`: Add portfolio constraints
- `slurm`: Adjust computational resources

## Troubleshooting

### Signal file not found
- Ensure `python src/create_signal.py` was run successfully
- Check that `data/signal.parquet` exists

### Backtest results missing
- Check that `python src/run_backtest.py` completed
- Verify `data/weights.parquet` exists
- Check logs in the `logs/` directory

### Data loading errors
- Verify CRSP data is available
- Check date ranges are valid
- Ensure required columns are available

## Environment

- Python 3.13+
- Dependencies: See `pyproject.toml`

Install dependencies:
```bash
pip install -e .
```

Or with uv:
```bash
uv sync
```

## Next Steps

1. Develop your signal in `app/signal_research.py`
2. Implement finalized logic in `src/create_signal.py`
3. Run the full pipeline to see results
4. Iterate and refine your approach

---

**Note**: This is a template project. Customize `create_signal.py` with your unique signal logic, then use the workflow above to research and backtest your ideas.
