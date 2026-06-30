# pydelay

**pydelay** is a Python package for simulating and analysing network time delays. It supports two statistical delay models — a Gaussian long-distance model and an exponential short-distance model — and produces publication-ready plots of the delay distribution and signal pulse train.

## Installation

```bash
pip install pydelay
```

## Quick Start

### As a library

```python
import pydelay

# Run a long-distance (Gaussian) delay simulation
result = pydelay.delay("l", n_packets=5000, simulation_time=120.0, seed=42)

print(f"Mean delay : {result.mean_delay:.3f} s")
print(f"Std  delay : {result.std_delay:.3f} s")
print(f"Min  delay : {result.min_delay:.3f} s")

# Plot the results
fig, axes = pydelay.plot(result, show=True)

# Save the figure
pydelay.plot(result, save="delay_plot.png")
```

### As a command-line tool

```bash
# Long-distance model, default settings
pydelay l

# Short-distance model, custom packet count and seed
pydelay s --npack 10000 --stime 60 --seed 0

# Skip writing the CSV output file
pydelay l --no-csv
```

## Delay Models

| Flag | Model | Distribution |
|------|-------|-------------|
| `l`  | Long-distance | Gaussian  μ = 1.50 s, σ = 0.50 s |
| `s`  | Short-distance | Exponential  min ≈ 1 ms, mean ≈ 2 ms |

## API Reference

### `pydelay.delay(model, *, n_packets, simulation_time, seed, output_csv)`

Run the delay simulation and return a `SimulationResult`.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `model` | `"l"` or `"s"` | — | Delay model |
| `n_packets` | `int` | `5000` | Number of packets to simulate |
| `simulation_time` | `float` | `120.0` | Time horizon in seconds |
| `seed` | `int \| None` | `None` | Random seed for reproducibility |
| `output_csv` | `str \| Path \| None` | `"delay_data.csv"` | Path to save CSV; `None` to skip |

### `pydelay.plot(result, *, zoom_start, zoom_stop, show, save)`

Plot the pulse train and delay distributions.

Returns `(fig, (ax_signal, ax_dist, ax_dist2))`.

### `SimulationResult`

| Attribute | Description |
|-----------|-------------|
| `.send_time` | Packet send timestamps (ndarray) |
| `.simulated_delay` | Sampled delays (ndarray) |
| `.pulse_time` / `.pulse_values` | Pulse train arrays |
| `.min_delay` | Minimum delay |
| `.mean_delay` | Mean delay |
| `.std_delay` | Standard deviation of delay |
| `.simdelay` | Dict of `{time, signals}` for external use |

## Requirements

- Python ≥ 3.10
- numpy ≥ 1.24
- matplotlib ≥ 3.7
- scipy ≥ 1.10

## License

MIT © Parham Kebria