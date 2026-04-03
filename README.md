# AI_HW1_314505009
# STS Smartphone IMU Dataset

## 1. Overview

This dataset supports **four-class classification** of **Sit-to-Stand (STS)** execution styles from **smartphone IMU** time series (triaxial accelerometer + triaxial gyroscope). Trials are organized in `data/metadata.csv` with one **raw CSV signal file** per trial.

The dataset was built for **NYCU AI Capstone (Spring 2026) — Project 1**: *construct and analyze your own dataset* (not an off-the-shelf public classification set).

---

## 2. Task

**4-class classification**

| Label       | Meaning (short)                                      |
|------------|-------------------------------------------------------|
| `normal`   | Smooth, continuous stand-up                         |
| `slow`     | Intentionally slower stand-up                        |
| `pause`    | Brief stop / hesitation during the transition        |
| `unstable` | Noticeable sway or loss of balance after standing   |

---

## 3. Data Type

- **Format:** CSV time series per trial  
- **Accelerometer (m/s² scale in generated data):** `acc_x`, `acc_y`, `acc_z`  
- **Gyroscope (rad/s):** `gyro_x`, `gyro_y`, `gyro_z`  
- **Time:** `seconds_elapsed` (uniform sampling)  

Column names follow `out/spec.json` and match the pipeline in `run/simulate_data.py` / `run/run_experiments.py`.

---



## 4. Data and Composition

Counts below are from **`data/metadata.csv`** in this repo.

| Item | Value |
|------|------:|
| **Total trials** | **200** |
| **Subjects (IDs)** | **5** (`S01`–`S05`) |
| **normal** | **50** |
| **slow** | **50** |
| **pause** | **50** |
| **unstable** | **50** |
| **Simulated (this release)** | **200** |
| **Real phone capture (this release)** | **0** |

**Subject distribution:** trials are spread across subjects (see `subject_id` in `metadata.csv`); group-wise splits (e.g., by subject) are recommended for evaluation.

**Note on “samples” for ML:** each trial CSV is one continuous recording. Downstream experiments use **sliding windows** (e.g. 2.56 s window, 50% overlap at 50 Hz → multiple windows per trial). The exact number of windows depends on `cfg/experiment.json` and is produced by `run/run_experiments.py`.

---

## 5. Collection Conditions

Aligned with the intended real-collection protocol:

- **Placement:** front pants pocket (`phone_placement` in metadata)  
- **Orientation:** consistent phone orientation when possible (`phone_orientation` in metadata)  
- **Duration:** **~6 s** per trial (`duration_s ≈ 6.0`)  
- **Nominal protocol segment (documentation):**  
  - **0–2 s:** sitting still  
  - **2–5 s:** stand-up motion  
  - **5–6 s:** standing still  

**Sampling:** target **50 Hz** (`sampling_rate_setting_hz` / `sampling_rate_est_hz`).

---

## 6. Label Definition

| Label | Definition |
|-------|------------|
| `normal` | Smooth, continuous stand-up |
| `slow` | Intentionally slower stand-up |
| `pause` | Brief stop or rhythm break during the transition |
| `unstable` | Visible sway or instability after standing |

---

## 7. File Format (CSV)

Each trial file contains:

| Column | Description |
|--------|-------------|
| `seconds_elapsed` | Time since trial start (s) |
| `acc_x`, `acc_y`, `acc_z` | Accelerometer |
| `gyro_x`, `gyro_y`, `gyro_z` | Gyroscope |

See also `out/spec.json` for the canonical column list.

---

## 8. Repository File Structure (actual layout)

```
data/
  metadata.csv              # trial index: labels, subject_id, paths, notes
  raw/
    real/                   # CSV files per trial (name is historical; see §4)
      T*_S*__<label>.csv    # e.g. T000001__S04__normal.csv
```

There is **no** `data/normal/` … split by class folder in this project; **class is given in the filename and in `metadata.csv`**.

---

## 9. Example

- **Metadata row:** open `data/metadata.csv` and pick any `trial_id`.  
- **Signal file:** use the `raw_file_path` field, e.g.  
  `data/raw/real/T000001__S04__normal.csv`

---

