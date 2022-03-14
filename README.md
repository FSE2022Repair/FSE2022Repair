# FSE2022Repair
A PyTorch Implementation of "Neural Program Repair with Typed Syntax"

# Introduction
Automated program repair (APR) aims to reduce the effort for software development. With the development of deep learning, lots of DL-based APR approaches have been proposed using an encoder-decoder architecture. Despite the promising performance, these models share one same limitation: generating lots of invalid patches. The main reason for this phenomenon is that the existing models do not take the typing constraint and accessible variable information into consideration.

In this paper, we propose, Tech, a relation-aware attention-based model based on the typed syntax graph. We first integrate the typing information with the syntax to generate the typed AST for code. Based on T-AST, we propose a new representation of code, Typed Syntax Graph (T-Graph), containing the typing constraints and the variable information. Finally, tech adopts the relation-aware attention layer to combine global reasoning over the code and variables with structured reasoning over T-AST.

The experiment was conducted on three benchmarks, 393 bugs from {Defects4J v1.2}, 444 additional bugs from {Defects4J v2.0} and 40 bugs from {QuixBugs}. Our results show that tech repairs 64, 32, and 27 bugs on these benchmarks respectively and outperforms the existing APR approaches on all benchmarks. The further analysis also shows that tech tends to generate more valid patches than the existing DL-based APR approaches with the typing and variable information.


# The Main File Tree

```
.
├── generation: code for patch generation
│   ├── bugs-QuixBugs
│ 	├── location
│ 	├── location2
│ 	└── tesetDefect4j.py
├── model: code for neural model
│   ├── data
│   ├── Model.py
│   └── relAttention.py
├── train: code for training the model
│   ├── run.py
│   ├── Dataset.py
│   └── SearchNode.py
├── validation: code for patch validation
│   ├── patches-all: plausible patches
│   ├── Dataset.py
│   └── SearchNode.py
```
# Dataset
## Train set
The raw data https://drive.google.com/drive/folders/1ECNX98qj9FMdRT2MXOUY6aQ6-sNT0b_a?usp=sharing from [Recoder](https://github.com/pkuzqh/Recoder).
## Test set
### [Defects4J](https://github.com/rjust/defects4j)
### [QuixBugs](https://github.com/jkoppel/QuixBugs)

# Usage

## Train a New Model
```python
CUDA_VISIBLE_DEVICES=0,1 python3 train/run.py train
```
The saved model is ```checkModel/best_model.cpkt```.

## Test the Model
### Generate Patches for Defects4J v1.2 with Ochiai by
```python
CUDA_VISIBLE_DEVICES=0 python3 generation/testDefect4j.py bugid
```

The generated patches are in folder ```generation/patch-all/patch/``` in json.

### Generate Patches for Defects4J v1.2 with groundtruth by
```python
CUDA_VISIBLE_DEVICES=0 python3 generation/testDefect4j1.py bugid
```

The generated patches are in folder ```generation/patch-all/patchground/``` in json.

### Generate Patches for Defects4J v2.0 by
```python
CUDA_VISIBLE_DEVICES=0 python3 generation/testDefects4j2.py bugid
```

The generated patches are in folder ```generation/patch-all/patch2``` in json.

### Generate Patches for QuixBugs by
```python
CUDA_VISIBLE_DEVICES=0 python3 generation/testQuixbug.py bugid
```

The generated patches are in folder ```generation/patch-all/patchQuix``` in json.

### Validate Patches for Defects4J v1.2 with Ochiai
```python
python3 validation/repair.py bugid
```

The results are in folder ```validation/patch-all/patches/``` in json.

### Validate Patches for Defects4J v1.2 with groundtruth
```python
python3 validation/repair1.py bugid
```

The results are in folder ```validation/patch-all/patchesground/``` in json.

### Validate Patches for Defects4J v2.0 with Ochiai
```python
python3 validation/repair2.py bugid
```

The results are in folder ```validation/patch-all/patches2/``` in json.

### Validate Patches for QuixBugs with Ochiai
```python
python3 validation/repairqfix.py bugid
```

The results are in folder ```validation/patch-all/patchesqfix/``` in json.

### Gnerated Patches
The generated patches are in the folder [patches-all](https://github.com/FSE2022Repair/FSE2022Repair/tree/main/validation/patches-all).


# Dependency
* Python 3.7
* PyTorch 1.3
* Defects4J
* Java 8
* docker
* nvidia-docker

