# How to find tune the LLama-3 on CSC



### Step 1: Git clone 

```bash
git clone https://github.com/meta-llama/llama3.git
```

### Step 2: Ask Meta for download url 

Just sign every thing they give to you in [here](https://llama.meta.com/docs/getting_the_models), 
and then your email will receive a url looks like this :

```
https://download6.llamameta.net/*?Policy=eyJTdGF0ZW1lbnQiOlt7InVuaKyerSceriZzRzZDg3aGpuOTN6dHlidmZ0czdwZnJwIiwiUmVzb3VyY2UiOiJodHRwczpcL1wvZG93bmxvYWQ2LmxsYW1hbWV0YS5uZXRcLyoiLCJDb25kaXRpb24iOnsiRGF0ZUxlc3NUaGFuIjp7IkFXUzpFcG9jaFRpbWUiOjE3MjAwODg2MTN9fX1dfQ__&Signature=JEijXnVR0nCYAYRXOCojIjKdQqtIJ039olWCfAD6nXGv-dV3ZcGGr5WSg7qf4Cfmn57k7uLHMbVNWRSLKcKernAHOWYohEbYVhGzwAPPIz7MNogxojyuwCktQhpH4hriY5qP2uX9NBI4zpOljS0mySV79UKUwKyUlJCwZnbjdX6TnzBYCIYmeS7Cmg%7EoTC3Xs8b18j643M%7EU1UDVPe7DQnvaEJWmskXV6puikRMwezIKj5OtCIVL1v6lYCQ10fNaB8VipzfvWiU0Mla1%7EjsGcadI82bO3CZ4GrEm-6yNKFJeUiJHIVCWjoCnEKdXJLf-3ahu8uz3ksv79--eQwC6XA__&Key-Pair-Id=K15QRJLYKIFSLZ&Download-Request-ID=1871090326722854
```

type in this command to download the pre-train model, 
```bash
cd llama3
./download.sh 
```

The script will ask you to paste the url link
```
Enter the URL from email:
```


### Step 3: Create Conda container  

This is procesure is the same as [Use CSC (Puhti or Mahti)](https://timbrist.github.io/workflow/hcp_csc)

**Step 3.1**: Create a environment file
```bash
export CW_DEBUG_KEEP_FILES="/projappl/project_xxxxxxx/llama3"
vim llama3_env.yml
```

**Step 3.2**: Add the following content
```
name: llama3
channels:
  - defaults
  - conda-forge
dependencies:
  - python=3.10
  - pip
  - pip:
    - -e /projappl/project_2010795/llama3
```
**Step 3.1**: Run the containerizer 
```bash
conda-containerize new --prefix ./llama3_env llama3_env.yml
```

### Step 4: Write a script to test it.


```
#!/bin/bash
#SBATCH --job-name=llama_test
#SBATCH --account=project_xxxxxxx
#SBATCH --partition=gputest
#SBATCH --cpus-per-task=10
#SBATCH --mem-per-cpu=8000
#SBATCH --gres=gpu:v100:1
#SBATCH --cpus-per-task=10
#SBATCH --mem-per-cpu=8000
#SBATCH --gres=gpu:v100:1

export PATH="/projappl/project_xxxxxxx/llama3/llama3_env/bin:$PATH" 
export HF_HOME=/scratch/project_xxxxxxx/cache
export CKPT_DIR=/scratch/project_xxxxxxx/Meta-Llama-3-8B

torchrun --nproc_per_node 1 example_chat_completion.py \
    --ckpt_dir ${CKPT_DIR}  \
    --tokenizer_path ${CKPT_DIR}/tokenizer.model \
    --max_seq_len 512 --max_batch_size 6
```


```bash
sbatch run_llama3_test.sh
tail -f slur[tab,tab]
```