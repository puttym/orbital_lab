# Classical Orbital Elements (COE)

This module computes **Classical Orbital Elements (COE)** from a given  
state vector (Cartesian position and velocity).

It is designed to be:

- **YAML-driven** (easy batch testing)
- **CLI executable**
- **Numerically transparent**
- **Modular for future expansion**

## How to Run

From the project root directory:

```bash
python -m src.coe.main --input data/coe_input_validation.yaml --output data/coe_output_results.yaml
```

### The program

- Reads input YAML
- Computes orbital elements
- Writes full-precision results to output YAML
- Prints a rounded table to the console

## Project Structure

```shell
orbital_lab/
│
├─ src/
│ └─ coe/
│ ├─ main.py
│ ├─ core.py
│ ├─ helpers.py
│ ├─ io_utils.py
│ ├─ messages.py
│ └─ init.py
│
├─ data/
│ ├─ coe_input_validation.yaml
│ ├─ coe_geometry_edge_cases.yaml
│ └─ coe_output_results.yaml
│
├─ tests/
│ └─ test_coe.py
│
├─ docs/
│ └─ orbital_mechanics_configurables.md
│
├─ configs/
│ ├─ main_config.yaml
│ ├─ orbit_config.yaml
│ └─ numerics_config.yaml
│
├─ README.md
└─ .gitignore
```

## File Responsibilities

### `src/coe/`

- **main.py** – CLI entry point and orchestration  
- **core.py** – Classical orbital element computation logic  
- **helpers.py** – Vector math and utility functions  
- **io_utils.py** – YAML read/write and table formatting  
- **messages.py** – Warning and error message templates  
- **init.py** – Module initializer  

---

### `data/`

- **coe_input_validation.yaml** – Input vectors for validation scenarios  
- **coe_geometry_edge_cases.yaml** – Inputs targeting orbital geometry limits  
- **coe_output_results.yaml** – Full-precision output written by the program  

---

### `tests/`

- **test_coe.py** – Automated tests verifying computation correctness  

---

### `docs/`

- **orbital_mechanics_configurables.md** – Design and configuration notes  

---

### `configs/`

- **main_config.yaml** – Global project configuration  
- **orbit_config.yaml** – Physical/domain configuration  
- **numerics_config.yaml** – Numerical tolerances and solver settings  

## Input YAML Format

The input YAML file contains **one or more state vector cases**.  
Each case is processed independently.

### Required Fields per Case

| Key             | Description                    | Units |
| --------------- | ------------------------------ | ----- |
| `name`          | Identifier for the case        | –     |
| `position_km`   | Position vector `[x, y, z]`    | km    |
| `velocity_km_s` | Velocity vector `[vx, vy, vz]` | km/s  |

### Optional Fields

| Key          | Description                          | Default |
| ------------ | ------------------------------------ | ------- |
| `frame`      | Reference frame label                | `ECI`   |
| `angle_unit` | Printed output unit (`deg` or `rad`) | `deg`   |

### Minimal Example

```yaml
cases:
  - name: simple_case
    position_km: [7000, 0, 0]
    velocity_km_s: [0, 7.5, 1.0]
```

### Extended Example

```yaml
cases:
  - name: inclined_orbit
    position_km: [6524.8, 6862.9, 6448.3]
    velocity_km_s: [4.901, 5.533, -1.976]
    frame: ECI
    angle_unit: deg
```

## Output Behavior

- **Output YAML →** Full-precision numeric values for further processing  
- **Console Table →** Rounded to three significant digits for readability  

**Note:** Printed values are rounded.  
The YAML file contains the accurate values.
