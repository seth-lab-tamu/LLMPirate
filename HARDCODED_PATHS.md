# Hardcoded File Paths - Configuration Guide

This document identifies all hardcoded file paths in the Python scripts that need to be made configurable for portability.

---

## Summary of Required Changes

### Quick Fix Approach
Create a `config.py` file with all paths as variables that can be easily modified by users.

### Recommended Solution
Use environment variables or a configuration file (JSON/YAML) for path settings.

---

## 1. Circuit Dataset Paths

### Path Pattern:
```
/home/grads/g/gohil.vasudev/Documents/Research/Generative_AI/LLM_for_IP_Piracy/all_GNN4IP_files/IP-Netlist-toy/
```

### Local Equivalent:
```
/home/matthewdelorenzo/LLM_for_IP_Piracy/all_GNN4IP_files/IP-Netlist-toy/
```

### Files Affected:
1. **`src/evaluate_piracy_using_cached_mapping_multiprocessing_v2.py`**
   - Line 540: `variants.extend(glob.glob("...all_GNN4IP_files/IP-Netlist-toy/"+design.upper()+"/*"))`
   - **Purpose**: Load test circuits (c432, c499, c880)

2. **`src/evaluate_piracy_using_cached_mapping_multiprocessing_v2_for_case_study.py`**
   - Line 540: Same pattern
   - **Purpose**: Load case study circuits (IBEX, AES, GPS, MOR1KX)

3. **`src/evaluate_piracy_using_cached_mapping_multiprocessing_v2_ablation_study_wo_SolC.py`**
   - Line 569: Same pattern
   - **Purpose**: Load circuits for ablation study

4. **`src/evaluate_GNN4IP_false_positive_rate.py`**
   - Line 529: `variants.extend(glob.glob("...all_GNN4IP_files/IP-Netlist-toy/"+design.upper()+"/*/topModule.v"))`
   - Line 604: Without `/topModule.v` suffix
   - **Purpose**: Evaluate GNN4IP false positive rates

5. **`src/count_num_gates.py`**
   - Line 84: `variants.extend(glob.glob("...all_GNN4IP_files/IP-Netlist-toy/"+design.upper()+"/*/*"))`
   - **Purpose**: Count gates in circuit variants

6. **`src/find_longest_paths.py`**
   - Line 82: Same pattern
   - **Purpose**: Analyze circuit depth

7. **`src/resyn_successful_circuits.py`**
   - Line 57: Same pattern
   - **Purpose**: Resynthesize successful obfuscated circuits

8. **`src_ablation_study_wo_SolA/GPT_communication_script_ablation_study_wo_SolA.py`**
   - Line 164: `variants.extend(glob.glob("...all_GNN4IP_files/IP-Netlist-toy/"+design.upper()+"/*"))`

9. **`src_ablation_study_wo_SolB/GPT_communication_script_ablation_study_wo_SolB.py`**
   - Line 156: Same pattern

### Recommended Fix:
```python
import os

# At top of file
REPO_ROOT = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
DATASET_PATH = os.path.join(REPO_ROOT, "all_GNN4IP_files", "IP-Netlist-toy")

# Then use:
variants.extend(glob.glob(os.path.join(DATASET_PATH, design.upper(), "*")))
```

---

## 2. GNN4IP Tool Paths

### A. Python Script Path
**Hardcoded:**
```
/home/grads/g/gohil.vasudev/Documents/Research/Generative_AI/LLM_for_IP_Piracy/all_GNN4IP_files/utils/hw2vec/examples/use_case_3_testing_for_LLM.py
```

**Local Equivalent:**
```
/home/matthewdelorenzo/LLM_for_IP_Piracy/all_GNN4IP_files/utils/hw2vec/examples/use_case_3_testing_for_LLM.py
```

**Files Affected:**
- `src/evaluate_piracy_using_cached_mapping_multiprocessing_v2.py` - Line 168
- `src/evaluate_piracy_using_cached_mapping_multiprocessing_v2_for_case_study.py` - Line 168
- `src/evaluate_piracy_using_cached_mapping_multiprocessing_v2_ablation_study_wo_SolC.py` - Line 183
- `src/evaluate_GNN4IP_false_positive_rate.py` - Line 168

### B. YAML Config Path
**Hardcoded:**
```
/home/grads/g/gohil.vasudev/Documents/Research/Generative_AI/LLM_for_IP_Piracy/all_GNN4IP_files/utils/hw2vec/examples/example_gnn4ip.yaml
```

**Local Equivalent:**
```
/home/matthewdelorenzo/LLM_for_IP_Piracy/all_GNN4IP_files/utils/hw2vec/examples/example_gnn4ip.yaml
```

**Files Affected:** Same as above (Line 169)

### C. Pretrained Model Path
**Hardcoded:**
```
/home/grads/g/gohil.vasudev/Documents/Research/Generative_AI/LLM_for_IP_Piracy/all_GNN4IP_files/utils/hw2vec/assets/pretrained_DFG_IP_Netlist_toy/
```

**Local Equivalent:**
```
/home/matthewdelorenzo/LLM_for_IP_Piracy/all_GNN4IP_files/utils/hw2vec/assets/pretrained_DFG_IP_Netlist_toy/
```

**Files Affected:** Same as above (Line 171)

### Recommended Fix:
```python
GNN4IP_SCRIPT = os.path.join(REPO_ROOT, "all_GNN4IP_files", "utils", "hw2vec", 
                             "examples", "use_case_3_testing_for_LLM.py")
GNN4IP_YAML = os.path.join(REPO_ROOT, "all_GNN4IP_files", "utils", "hw2vec", 
                           "examples", "example_gnn4ip.yaml")
GNN4IP_MODEL = os.path.join(REPO_ROOT, "all_GNN4IP_files", "utils", "hw2vec", 
                            "assets", "pretrained_DFG_IP_Netlist_toy")
```

---

## 3. SIM Tool Paths

### A. v2c Executable
**Hardcoded:**
```
/home/grads/g/gohil.vasudev/Documents/Research/Generative_AI/LLM_for_IP_Piracy/sim/v2c
```

**Local Equivalent:**
```
/home/matthewdelorenzo/LLM_for_IP_Piracy/sim/v2c
```

**Files Affected:**
- `src/sim_evaluation.py` - Line 8

### B. sim_text Executable
**Hardcoded:**
```
/home/grads/g/gohil.vasudev/Documents/Research/Generative_AI/LLM_for_IP_Piracy/sim/
```
(Used to construct: `executable = path + lang` where `lang` is like "sim_text")

**Local Equivalent:**
```
/home/matthewdelorenzo/LLM_for_IP_Piracy/sim/
```

**Files Affected:**
- `src/sim_evaluation.py` - Line 18

### Recommended Fix:
```python
SIM_DIR = os.path.join(REPO_ROOT, "sim")
V2C_EXECUTABLE = os.path.join(SIM_DIR, "v2c")
```

---

## 4. JPlag Tool Path

**Hardcoded:**
```
/home/grads/g/gohil.vasudev/Documents/Research/Generative_AI/LLM_for_IP_Piracy/jplag-5.0.0-jar-with-dependencies.jar
```

**Local Equivalent:**
```
/home/matthewdelorenzo/LLM_for_IP_Piracy/jplag-5.0.0-jar-with-dependencies.jar
```

**Files Affected:**
- `src/Jplag_evaluation.py` - Lines 5 and 37

### Recommended Fix:
```python
JPLAG_JAR = os.path.join(REPO_ROOT, "jplag-5.0.0-jar-with-dependencies.jar")
```

---

## 5. ABC Synthesis Tool Paths

### A. ABC Executable
**Hardcoded:**
```
/mnt/research/Rajendran_Jeyavijayan/Students/Gohil_Vasudev/Research/RL/AI_vs_AI/RL_for_breaking_all/abc_my/abc/abc
```

**Status:** ❌ **NOT FOUND** in repository
**Note:** This is an external tool that must be installed separately

**Files Affected:**
- `src/evaluate_piracy_using_cached_mapping_multiprocessing_v2.py` - Line 403
- `src/evaluate_piracy_using_cached_mapping_multiprocessing_v2_for_case_study.py` - Line 403
- `src/evaluate_piracy_using_cached_mapping_multiprocessing_v2_ablation_study_wo_SolC.py` - Line 418
- `src/resyn_successful_circuits.py` - Line 33
- `src/evaluate_GNN4IP_false_positive_rate.py` - Line 404 (commented out)

### B. NanGate Library Files
**Hardcoded:**
```
/mnt/research/Rajendran_Jeyavijayan/Students/Gohil_Vasudev/Research/RL/AI_vs_AI/RL_for_breaking_all/synthesis/libs_for_GNN4IP/NangateOpenCellLibrary_typical.lib
/mnt/research/Rajendran_Jeyavijayan/Students/Gohil_Vasudev/Research/RL/AI_vs_AI/RL_for_breaking_all/synthesis/libs_for_GNN4IP/NangateOpenCellLibrary_typical_reduced.lib
```

**Status:** ❌ **NOT FOUND** in repository
**Note:** These are library files needed for synthesis (only used if `using_resynthesis = True`)

**Files Affected:**
- `src/evaluate_piracy_using_cached_mapping_multiprocessing_v2.py` - Lines 156, 160
- `src/evaluate_piracy_using_cached_mapping_multiprocessing_v2_for_case_study.py` - Lines 156, 160
- `src/evaluate_piracy_using_cached_mapping_multiprocessing_v2_ablation_study_wo_SolC.py` - Lines 171, 175
- `src/resyn_successful_circuits.py` - Lines 13, 17
- `src/evaluate_GNN4IP_false_positive_rate.py` - Lines 156, 160 (commented out)

### Recommended Fix:
```python
# These should be configurable or use environment variables
ABC_EXECUTABLE = os.environ.get('ABC_PATH', 'abc')  # Falls back to system PATH
NANGATE_LIB = os.environ.get('NANGATE_LIB_PATH', None)
NANGATE_LIB_REDUCED = os.environ.get('NANGATE_LIB_REDUCED_PATH', None)

# Or fail gracefully if not found:
if using_resynthesis and not os.path.exists(ABC_EXECUTABLE):
    raise FileNotFoundError("ABC synthesis tool not found. Please install ABC or set ABC_PATH environment variable.")
```

---

## 6. Test/Example Paths (in __main__ sections)

### Files with hardcoded test paths:
1. **`src/characterize_circuit.py`** - Line 169
   ```python
   generic_version_of_current_circuit = "/mnt/research/.../all_GNN4IP_files/IP-Netlist-toy/C432/c432-BE280/topModule.v"
   ```

2. **`src_ablation_study_wo_SolA/characterize_circuit.py`** - Line 216
   (Same pattern)

3. **`src_ablation_study_wo_SolB/characterize_circuit.py`** - Line 216
   (Same pattern)

**Note:** These are only in `if __name__ == '__main__':` sections for testing, so lower priority to fix.

---

## 7. Successful Circuits Output Paths

**Hardcoded:**
```
/home/grads/g/gohil.vasudev/Documents/Research/Generative_AI/LLM_for_IP_Piracy/src/successful_circuits/
```

**Files Affected:**
- `src/count_num_gates.py` - Lines 150, 156
- `src/find_longest_paths.py` - Lines 127, 133
- `src/resyn_successful_circuits.py` - Lines 69, 74

**Note:** These directories were already cleaned up and don't exist anymore.

---

## Implementation Recommendations

### Option 1: Create `config.py` (Simplest)

Create `src/config.py`:
```python
import os

# Auto-detect repository root
REPO_ROOT = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))

# Dataset paths
DATASET_PATH = os.path.join(REPO_ROOT, "all_GNN4IP_files", "IP-Netlist-toy")

# GNN4IP paths
GNN4IP_SCRIPT = os.path.join(REPO_ROOT, "all_GNN4IP_files", "utils", "hw2vec", 
                             "examples", "use_case_3_testing_for_LLM.py")
GNN4IP_YAML = os.path.join(REPO_ROOT, "all_GNN4IP_files", "utils", "hw2vec", 
                           "examples", "example_gnn4ip.yaml")
GNN4IP_MODEL = os.path.join(REPO_ROOT, "all_GNN4IP_files", "utils", "hw2vec", 
                            "assets", "pretrained_DFG_IP_Netlist_toy")

# SIM tool paths
SIM_DIR = os.path.join(REPO_ROOT, "sim")
V2C_EXECUTABLE = os.path.join(SIM_DIR, "v2c")

# JPlag path
JPLAG_JAR = os.path.join(REPO_ROOT, "jplag-5.0.0-jar-with-dependencies.jar")

# ABC synthesis (external tools - use environment variables)
ABC_EXECUTABLE = os.environ.get('ABC_PATH', 'abc')
NANGATE_LIB = os.environ.get('NANGATE_LIB_PATH', None)
NANGATE_LIB_REDUCED = os.environ.get('NANGATE_LIB_REDUCED_PATH', None)

# Output directories
OUTPUT_DIR = os.path.join(REPO_ROOT, "src", "outputs")
TMP_DIR = os.path.join(REPO_ROOT, "src", "tmp")
```

Then in each file, add:
```python
from config import DATASET_PATH, GNN4IP_SCRIPT, GNN4IP_YAML, GNN4IP_MODEL, etc.
```

### Option 2: Environment Variables + `.env` file

Create `.env.example`:
```bash
# Repository paths (auto-detected if not set)
REPO_ROOT=/home/matthewdelorenzo/LLM_for_IP_Piracy

# External tools (required if using resynthesis)
ABC_PATH=/path/to/abc
NANGATE_LIB_PATH=/path/to/NangateOpenCellLibrary_typical.lib
NANGATE_LIB_REDUCED_PATH=/path/to/NangateOpenCellLibrary_typical_reduced.lib
```

Users copy to `.env` and modify for their setup.

### Option 3: Configuration File (Most Flexible)

Create `config.yaml`:
```yaml
paths:
  dataset: all_GNN4IP_files/IP-Netlist-toy
  gnn4ip:
    script: all_GNN4IP_files/utils/hw2vec/examples/use_case_3_testing_for_LLM.py
    yaml: all_GNN4IP_files/utils/hw2vec/examples/example_gnn4ip.yaml
    model: all_GNN4IP_files/utils/hw2vec/assets/pretrained_DFG_IP_Netlist_toy
  tools:
    sim_dir: sim
    jplag_jar: jplag-5.0.0-jar-with-dependencies.jar
    abc: /path/to/abc  # User must configure
    nangate_lib: /path/to/lib  # User must configure
```

---

## Priority List

### High Priority (Required for basic functionality):
1. ✅ **Dataset paths** - Used by all evaluation scripts
2. ✅ **GNN4IP paths** - Used by main detection
3. ✅ **SIM tool paths** - Used by SIM detection
4. ✅ **JPlag path** - Used by JPlag detection

### Medium Priority (Only if using resynthesis):
5. ⚠️ **ABC executable** - External tool, needs installation instructions
6. ⚠️ **NanGate libraries** - External files, needs download instructions

### Low Priority (Testing only):
7. 🔵 **Test example paths** - Only in `__main__` sections

---

## Files Summary Table

| File | Hardcoded Paths | Priority |
|------|----------------|----------|
| `evaluate_piracy_*_v2.py` | Dataset, GNN4IP, ABC | High |
| `sim_evaluation.py` | SIM tools | High |
| `Jplag_evaluation.py` | JPlag JAR | High |
| `count_num_gates.py` | Dataset | Medium |
| `find_longest_paths.py` | Dataset | Medium |
| `characterize_circuit.py` | Test path | Low |
| `resyn_successful_circuits.py` | Dataset, ABC | Medium |

---

## Next Steps

1. **Create `src/config.py`** with auto-detection of repository root
2. **Update all scripts** to import from config
3. **Create `.env.example`** for external tool paths
4. **Update README** with configuration instructions
5. **Add validation** to check if required files exist before running
