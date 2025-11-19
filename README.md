# Understanding and Detecting Scalability Faults

This is the artifact of the submitted manuscript *Understanding and Detecting Scalability Faults in Large-Scale Distributed Systems*. This artifact contains two parts: (1) raw data description and (2) experiment reproduction.

Source code for ScaleLens can be found [here](https://github.com/scalability-study/ScaleLens) and the Docker images for experiments can be found [here](https://hub.docker.com/r/scaleview/scaleview).

## 1. Raw Data Description  

This section provides scripts to generate all data presented in paper. Python package `pandas` is required ([install it](https://pandas.pydata.org/docs/getting_started/install.html)).

### 1.1 Scalability Fault Patterns (Section 2)

This section presents the raw data used for investigating code patterns of scalability faults. In this section, 444 scalability faults were analyzed and categorized into 4 main patterns and 11 sub-patterns.

A CSV file that contains all 444 scalability faults, as well as tags indicating their patterns, can be found [here](pattern-study/scalability-bugs.csv).

To generate table 1 in the paper, run the following command:

``` sh
REPO_DIR=$(git rev-parse --show-toplevel)
python $REPO_DIR/scripts/table-1-breakdown.py
```

To generate all data for the patterns in empirical study, run the following command:

``` sh
REPO_DIR=$(git rev-parse --show-toplevel)
python $REPO_DIR/scripts/pattern-analysis.py
```

### 1.2 Evaluation Data (Section 4)

In this section, we describe the raw data from our evaluation. For RQ1, we provide raw outputs from ScaleLens and ScaleCheck. For RQ2, we provide our analysis details. 


#### 1.2.1 Research Question 1: Effectiveness of ScaleLens and Comparison with Baseline

Raw outputs from ScaleLens are available [here](evaluation-data/scalelens-outputs). There are one json file for each system, listing all DCFs, their dimensions with relationship, and the anti-patterns they are associated with. 

ScaleCheck is our baseline to compare with ScaleLens. Raw outputs from ScaleCheck are available [here](evaluation-data/scalecheck-outputs). For each system, there is one file containing the list of DCFs detected by ScaleCheck.

#### 1.2.3 Research Question 2: Result Analysis

We provide our ablation analysis of the results from ScaleLens on the latest versions of Cassandra, HDFS, and Ignite. The analysis details are available [here](evaluation-data/ablation-study).



## 2. Experiment Reproduction

This section provides instructions to reproduce the experiments conducted in the study. This will involve running the ScaleLens pipeline ([source code](./scalelens/)) in Docker containers.


### 2.1 Run ScaleLens

All experiments are packaged as Docker images available on Docker Hub ([link](https://hub.docker.com/r/scaleview/scaleview)). To run an experiment, for example, with Cassandra 4.1.0, use the following command to pull and run the Docker image:

``` sh
docker pull scaleview/scaleview:CA-4.1.0
docker run -it scaleview/scaleview:CA-4.1.0
```

Then run the experiment with the following command:

``` sh
bash run.sh experiments/CA-4.1.0.yaml workspace
```

To run other experiments, replace `CA-4.1.0` with the experiment ID(`SYSTEM-VERSION`). All available experiments can be found in the `experiments` directory [here](./scalelens/experiments/).



### 2.3 Run Ablation Analysis

To replicate our ablation analysis (Table III), run the following command from this repository:

``` sh
REPO_DIR=$(git rev-parse --show-toplevel)
cd $REPO_DIR/evaluation-data/ablation-study/all-fragments && python run_stat.py
```

It lists statistics of all fragments without applying ScaleView. To see how ScaleLens narrows down to DCFs, run the following command:

``` sh
REPO_DIR=$(git rev-parse --show-toplevel)
cd $REPO_DIR/evaluation-data/ablation-study/scalelens && python run_stat.py
```
