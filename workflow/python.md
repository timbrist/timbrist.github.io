# The workflow of Python


## virtual environment

### Create a virtual environment with conda

```bash
conda create -n env_name python=3.8
```

### Activate the environment
```bash
conda activate env_name
```

After activate the environment, you would use the command ```pip install ``` .
or install the package from the ```requirement.txt``` file. 

```bash
pip install -r requirement.txt
```

## pytorch

### check if the pytorch use cuda

```bash
import torch
torch.cuda.is_available() # shall output: True
```
### check the version of pytorch
```bash
import torch
print(torch.__version__)
```

### running llama2 with pytorch issues 

#### torch.distributed.elastic.multiprocessing.errors.ChildFailedError

this error also mentioned [here](https://github.com/Vision-CAIR/MiniGPT-4/issues/237)

after running the script from llama2:

```bash 
torchrun --nproc_per_node 1 example_chat_completion.py \
    --ckpt_dir llama-2-7b-chat/ \
    --tokenizer_path tokenizer.model \
    --max_seq_len 512 --max_batch_size 4
```

the error :

```bash
> initializing model parallel with size 1
> initializing ddp with size 1
> initializing pipeline with size 1
[2024-03-26 16:50:50,221] torch.distributed.elastic.multiprocessing.api: [ERROR] failed (exitcode: -9) local_rank: 0 (pid: 11127) of binary: /home/yan/anaconda3/envs/llama2/bin/python
Traceback (most recent call last):
  File "/home/yan/anaconda3/envs/llama2/bin/torchrun", line 8, in <module>
    ...
  File "/home/yan/anaconda3/envs/llama2/lib/python3.10/site-packages/torch/distributed/launcher/api.py", line 268, in launch_agent
    raise ChildFailedError(
torch.distributed.elastic.multiprocessing.errors.ChildFailedError: 
======================================================
example_chat_completion.py FAILED
------------------------------------------------------
Failures:
  <NO_OTHER_FAILURES>
------------------------------------------------------
Root Cause (first observed failure):
[0]:
  time      : 2024-03-26_16:50:50
  host      : yan-ml
  rank      : 0 (local_rank: 0)
  exitcode  : -9 (pid: 11127)
  error_file: <N/A>
  traceback : Signal 9 (SIGKILL) received by PID 11127
======================================================
```



#### Dignose the issue 

~~for me it probabily run out of memoery. ~~
[20240326] I have the same error on HCP, 




### export environment configuration:

```
conda env export | grep -v "^prefix: " > environment.yml
```