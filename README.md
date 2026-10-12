# ProDriver Sim

ProDriver Sim is a Python desktop and command-line tool for estimating the electrical operating point of an LED driver and LED module from **JSON configuration curves**. Developed as a test-support project at LUX Dynamics. The current public source provides deterministic calculations; it is not a calibrated digital twin of all driver behaviors.

## Implemented in the current code

- Load driver and LED-module profiles from JSON.
- Interpolate module voltage against programmed module current, allowing for series/parallel quantities in the profile.
- Calculate DC output power: `Pout = Vmodule * Imodule`.
- Estimate driver efficiency from available power, load-percentage and voltage efficiency lookup curves, with limits of 50–98% in the source.
- Estimate AC input power as `Pin = Pout / efficiency`.
- Report **warnings** for input voltage, regulation voltage, driver power and LED/module current limits. The current `simulate()` function **does not automatically clamp** the operating point to a safe level.
- Use PyQt6 for a desktop interface, or run without PyQt6 using the command-line mode.

## Files

| Path | Purpose |
|---|---|
| `main.py` | Simulation, JSON parsing, CLI, and optional desktop UI |
| `BST1.json` | Example archived driver configuration |
| `HO7_4800lm.json`, `HO7_6400lm.json`, `HO7_1'_4800lm.json` | Archived LED module configuration files |
| `driver_sim.db` | Database present in archive; the current `main.py` does **not** query it |
| `main.spec` | PyInstaller recipe |

## Run

Python 3.10+ recommended. The command-line model uses only Python's standard library.

```bash
git clone https://github.com/kalarcon775/ProDriver-Sim.git
cd ProDriver-Sim
python main.py --driver BST1.json --module HO7_4800lm.json --current 0.6 --input-v 120
```

For the desktop interface, install PyQt6 and launch without CLI options:

```bash
python -m pip install PyQt6
python main.py
```

No SciPy, NumPy or Matplotlib import appears in the current `main.py` source. That does **not** guarantee this legacy application has been tested with every Python/PyQt environment.

## What is not implemented or not independently validated

Some previously documented features describe future or different versions. The **current source does not** include:
- Randomized AC grid-voltage sampling or outlier rejection.
- Independent mains/driver AC-loss or fixed-loss calculation.
- Applied thermal derating even though some JSON profiles contain thermal curves.
- Calibration, device-specific uncertainty propagation, or automated measurement-file correlation.
- Guaranteed power-limit clipping; out-of-range operating points are reported as issues.

The archived curve points may have been drawn from earlier engineering work, but their original instrument measurements, fit quality, calibration, and uncertainty have not been independently recovered in this repository. Predictions are estimates, **not verified bench results**. Do not use an out-of-range result as a safe fixture configuration.

## Engineering recovery documentation

The [portfolio recovery directory](https://github.com/kalarcon775/kalarcon775.github.io/tree/main/projects/prodriver) contains an annotated architecture, calculations, verification matrix, and independently recreated offline tests. It links to this original source without duplicating corporate configuration files.

## Attribution

Original application and archived public profiles by Kailani Alarcon, developed during work at LUX Dynamics. Later engineering recovery work is identified separately.
