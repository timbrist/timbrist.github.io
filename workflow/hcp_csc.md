# Use CSC (Puhti or Mahti)

Thanks to CSC-Tieteen tietotekniikan keskus Oy, Students in Finland have change to run high performance computer for free. But they change the system frequently. By the time you read my post, I am not sure this page will work as the same.

This page is my workflow for using GPU cluster on CSC [July 2. 2024].

Login to [Mahti](https://www.mahti.csc.fi/public/) or [Puhti](https://www.puhti.csc.fi/public/).


## Inference with [Video-LLaVa](https://github.com/PKU-YuanGroup/Video-LLaVA)

Choose "Login node shell" after your login. 

### Step 1: Check your workspace

```bash 
[username@mahti-login ~]$ csc-workspaces
```
You will see 3 folder you have: 

```bash 
Disk area               Capacity(used/max)  Files(used/max)  Project description  
----------------------------------------------------------------------------------
Personal home folder
----------------------------------------------------------------------------------
/users/username               500k/10G         .11k/100k
Project applications 
----------------------------------------------------------------------------------
/projappl/project_xxxxxxx     17.59G/50G        0k/100k    
Project scratch 
----------------------------------------------------------------------------------
/scratch/project_xxxxxxx      54.47G/1T         4.01k/1000k  
```

Usually **/scratch/project_xxxxxx** is used for large file, **/projappl/project_xxxxxxx** is used for your actual project.


### Step 2: Clone [Video-LLaVa](https://github.com/PKU-YuanGroup/Video-LLaVA) to your project folder

```bash 
cd /projappl/project_xxxxxxx
git clone https://github.com/PKU-YuanGroup/Video-LLaVA
cd Video-LLaVA
```

### Step 3: Containerization on GPU Cluster

Don't just try to use **Conda**, Cluster usually use their own Containerization. In CSC case, they use [Tykky](https://docs.csc.fi/computing/containers/tykky/). Check the installation, sometime they update.

#### Step 3.1 : Run the application tykky

```bash 
module load tykky
```

#### Step 3.2 Write the environment configure files

make sure you add all the dependencies you need, otherwise when you want to add other dependencies it will take FOREVER to update.

**Step 3.2.1**: Create a Conda environment YAML file (environment.yml)

```bash
export CW_DEBUG_KEEP_FILES="/projappl/project_xxxxxxx/Video-LLaVA"
vim videollava_evn.yml
```

**Step 3.2.1**: Add the requirements base on installation instruction [Video-LLaVa](https://github.com/PKU-YuanGroup/Video-LLaVA) provided 

This the most difficult part, since CSC are using their own parameters. 
```
'[/MAHTI_TYKKY_xj2Awum/miniconda/envs/env1/bin/python', '-m', 'pip', 'install', '-U', '-r', '/MAHTI_TYKKY_xj2Awum/condaenv.5mlbkjw8.requirements.txt', '--exists-action=b']
```

```bash
name: videollava
channels:
  - defaults
  - conda-forge
dependencies:
  - python=3.10 
  - pip
  - pip: 
    - pip==22.0
    - -e /projappl/project_xxxxxxx/Video-LLaVA
    - -e "/projappl/project_xxxxxxx/Video-LLaVA/[train]"
```

We have to deal with the rest of the requirement after we install the environment.

#### Step 3.3 Install the environment

**Step 3.3.1**: Create a folder to store the package

```bash
mkdir videollava_evn
```
**Step 3.3.2**: Run container command

```bash
conda-containerize new --prefix ./videollava_evn videollava_evn.yml
```

#### Step 3.4 Continue to install rest of the package 
If you're luck enough, you can install everything in the **Conda** yml file, but a lot of time you will end of here, constantly install shits. 

**Step 3.4.1**: Install the rest of the package we cannot install during the Conda

```bash
vim restpackages.sh
```

**Step 3.4.2**: Add the content

```
pip install flash-attn --no-build-isolation \
pip install decord opencv-python git+https://github.com/facebookresearch/pytorchvideo.git@28fe037d212663c6a24f373b94cc5d478c8c1a1d
```

**Step 3.4.3**: Run the conda update command

firstly load cuda, because package: **flash-attn** needs nvcc.
secondly update the conda container
```bash
module load cuda 
conda-containerize update --post-install post.sh /path/to_install
```


#### Step 3.5 Run the [Video-LLaVa](https://github.com/PKU-YuanGroup/Video-LLaVA) on Cluster with script



**Step 3.5.1**: Create a [GPU batch job script](https://docs.csc.fi/computing/running/creating-job-scripts-mahti/)

NOTE: change the directory to project.
```bash
vim run_videollava.sh
```

**Step 3.5.2**: add the rest of the content
remember change the xxxxxxx to your project folder.

The sbatch means that we are using 
```
#!/bin/bash
#SBATCH --job-name=videollava_test
#SBATCH --account=project_xxxxxxx
#SBATCH --partition=gpusmall
#SBATCH --gres=gpu:a100:1
#SBATCH --time=00:15:00
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=32  

module load myprog/1.2.3

export PATH="/projappl/project_xxxxxxx/Video-LLaVA/videollava_evn/bin:$PATH" 
export HF_DATASETS_CACHE=/scratch/project_xxxxxxx/videollava_cache
export XDG_CACHE_HOME=/scratch/project_xxxxxxx/videollava_cache
export PIP_CACHE_DIR=/scratch/project_xxxxxxx/videollava_cache
export TRANSFORMERS_CACHE=/scratch/project_xxxxxxx/videollava_cache
export HF_HOME=/scratch/project_xxxxxxx/videollava_cache

CUDA_VISIBLE_DEVICES=0 python -m videollava.serve.cli --model-path "LanguageBind/Video-LLaVA-7B" --file "/scratch//project_xxxxxxx/BDD/samples-1k/videos/08008acf-4c0ddc8b.mov" --load-4bit

```

**Step 3.5.3** : Submit to the queue. [instruction](https://docs.csc.fi/computing/running/submitting-jobs/)

```bash
sbatch run_videollava.sh
```

If you job is running.  

Attach to the job

```bash
tail -f slurm-3578575.out
```