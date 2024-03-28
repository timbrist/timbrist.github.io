# Use CSCS 


## Create Containerization based on [Tykky](https://docs.csc.fi/computing/containers/tykky/)


### Run the application tykky
```bash 
module load tykky
```

### write the environment configure files

make sure you add all the dependencies you need, otherwise when you want to add other dependencies it will take forever to update.

here is my absolute path ```/scratch/project_2009655/projects/llama/container```

```bash
vim llama2_env.yml
```
here I am going to use the example that to run llama2-7B-chat

```bash
name: llama2
channels:
  - defaults
  - pytorch
dependencies:
  - python=3.10
pip:
  - torch=2.2.0 
  - torchvision=0.17.0 
  - torchaudio=2.2.0
  - cudatoolkit=11.8
  - fairscale
  - fire
  - sentencepiece
```

```
create a folder to store the package

```bash
mkdir llama2_env
```

```bash

conda-containerize new --prefix ./llama2_env llama2_env.yml
```

I have no idea why 
```bash 
export PATH="/scratch/project_2009655/projects/llama/container/llama2_env/bin:$PATH" 
export XDG_CACHE_HOME=/scratch/project_2009655/cache
pip install torch==2.2.0 torchvision==0.17.0 torchaudio==2.2.0 --index-url https://download.pytorch.org/whl/cu118
NOTE: change the directory to project.
```bash
cd ..
vim run_llama2.sh

#!/bin/bash
#SBATCH --job-name=llama_test
#SBATCH --account=project_2009655
#SBATCH --partition=gputest
#SBATCH --time=00:15:00
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=10
#SBATCH --mem-per-cpu=8000
#SBATCH --gres=gpu:v100:1
export PATH="/scratch/project_2009655/projects/llama/container/llama2_env/bin:$PATH" 
export HF_HOME=/scratch/project_2009655/cache
torchrun --nproc_per_node 1 example_chat_completion.py \
    --ckpt_dir llama-2-7b-chat/ \
    --tokenizer_path tokenizer.model \
    --max_seq_len 512 --max_batch_size 4
```

```bash
sbatch run_llama2.sh
```