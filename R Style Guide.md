
# R Code Style & Contribution Guide  
**For: Maypop Labs**  
_Last updated: June 2025_

---

## Directories and Local Paths

- Files may have hardcoded directory names that may be changed as needed for local work.
- As the project progresses, a more robust system will be developed.

---

## File Naming Conventions

- **Primary scripts** are numbered by execution order:  
  Example: `01 - Loading, Aggregation, and Quality Control.R`, `13 - Attractor Analysis - Part 03.R`

- **Helper scripts** use descriptive names without numbers:  
  Example: `boolean_helpers.R`

- **Use Pascal Case or Title Case** for multi-word script names, separated by spaces and dashes.

- **Output files** use consistent suffixes and patterns:  
  - `*.rds` for R objects  
  - `*.tsv` for tables  
  - `*.png` for figures  

---

## Variable Naming

### General Rules

- Use **camelCase** for all variable and function names: `cellOrder`, `boolnetFile`, `ageScore`.
- Avoid snake_case and dot.case (e.g., `my_var`, `my.var`) for new variables.
- Use **short names for global or persistent variables** (`cds`, `gMerged`, `boolnet`) and **longer descriptive names for local variables** in limited-scope functions (`decodedVec`, `entropyResults`, `entropyDf`).

### Naming Prefixes (Optional but Encouraged)

| Prefix | Meaning                        | Example           |
|--------|--------------------------------|-------------------|
| `cds`  | Monocle3 CellDataSet object    | `cds`, `cdsPath`  |
| `rds`  | File path to RDS object        | `degReadFile`     |
| `mat`  | Matrix object                  | `matBin`, `matExpr` |
| `df`   | Data frame object              | `geneDf`, `entropyDf` |
| `bool` | BoolNet object or logic        | `boolnet`, `boolRules` |

---

## Commenting Style

### Section Headers
Use box-style comments for section separation and clarity:

```r
# =============================================================================
# 05 - Smoothing.R
# Purpose: Smooth gene expression using pseudotime sliding window
# =============================================================================
```

### Inline Comments
Use descriptive, aligned comments where needed, but avoid clutter:
```r
winSize <- floor(0.10 * nCells)  # 10% of total cells
```

### Message Logging
Standardize terminal output using `message()` or `cat()`:
```r
message("Loading Monocle3 Object")
cat("Initial Aging Score:", initialScore, "\n")
```

---

## Function Naming and Placement

- All **custom functions** must use **camelCase**, be **self-contained**, and placed in `boolean_helpers.R`.

- Organize `boolean_helpers.R` with **alphabetical ordering** of functions and use `# =============================================================================` to visually separate them.

- Clearly label helper functions with parameter descriptions:
```r
# ----------------------------------------------------------------------
# computeAgeScore
# Computes the scalar projection of a state vector onto the aging axis
# ----------------------------------------------------------------------
```

---

## Modular Design Philosophy

Each script:

- Performs **one major processing step**
- Loads only the **minimal required data** using safe `if (file.exists(...))` guards
- Outputs standard `.rds`, `.tsv`, or `.png` files
- Communicates through **flat file outputs**, enabling replacement of any module

---

## Graph and Plot Standards

- Use `ggplot2` for all data visualization.
- Use **generic gene labels** (e.g., `Gene1`, `Gene2`, etc.) for figures to preserve privacy/IP.
- Apply `theme_minimal()` to all plots.
- Save all plots using `ggsave()` to `.../R Plots/`.

---

## Best Practices

- **Avoid hardcoded values** when possible; parameterize values like `cellType`, `trajectory`, and file paths.
- **Gracefully handle missing files** using `if (file.exists(...))`.
- **Use `stop()` for critical failures** and `warning()` for recoverable issues.
- **Limit side effects:** Scripts shouldn’t alter global environment or external states unnecessarily.
- **Consistent ordering of logic:** `load → process → output → message`.

---

## Naming in Perturbation Experiments

| Mode            | Label in Files |
|------------------|----------------|
| Knockdown        | `_0.rds`       |
| Overexpression   | `_1.rds`       |
| KD+KD            | `_KD_KD.rds`   |
| OE+OE            | `_OE_OE.rds`   |
| KD+OE            | `_KD_OE.rds`   |

---

## Checklist for New Scripts

Before adding a new script to the pipeline:
- [ ] Uses camelCase and descriptive names
- [ ] Starts with formatted comment header
- [ ] Loads data with `file.exists()` checks
- [ ] All functions placed in `boolean_helpers.R` unless script-specific
- [ ] Uses consistent output file naming
- [ ] Saves outputs in appropriate `/rds`, `/tsv`, or `/Plots` directories
- [ ] Tested locally with `saveResults <- TRUE`
