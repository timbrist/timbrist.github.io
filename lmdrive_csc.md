# How to solve the issues that running LMDrive on CSC Puhti

Recently, I try to run the [LMDrive](https://github.com/opendilab/LMDrive/) on high-performance computing(HPC) [puhti of csc](https://www.puhti.csc.fi/). I was able to run the LMDrive script on my personal computer with Ubuntu22.04. But because I am not familar with HPC, I spend almost two days to solve the dependencies on puhti. 


## RuntimeError: Not compiled with CUDA support
if You ran into this Error after Running the route, you probablily install wrong torch-scatter.

```Shell
Traceback (most recent call last):
  File "/projappl/project_2009655/LMDrive/leaderboard/leaderboard/scenarios/scenario_manager.py", line 155, in _tick_scenario
    ego_action = self._agent()
  File "/projappl/project_2009655/LMDrive/leaderboard/leaderboard/autoagents/agent_wrapper.py", line 82, in __call__
    return self._agent()
  File "/projappl/project_2009655/LMDrive/leaderboard/leaderboard/autoagents/autonomous_agent.py", line 115, in __call__
    control = self.run_step(input_data, timestamp)
  File "/PUHTI_TYKKY_7d8FtWn/miniconda/envs/env1/lib/python3.8/site-packages/torch/utils/_contextlib.py", line 115, in decorate_context
    return func(*args, **kwargs)
  File "/projappl/project_2009655/LMDrive/leaderboard/team_code/lmdriver_agent.py", line 508, in run_step
    image_embeds = self.net.visual_encoder(input_data)
  File "/PUHTI_TYKKY_7d8FtWn/miniconda/envs/env1/lib/python3.8/site-packages/torch/nn/modules/module.py", line 1501, in _call_impl
    return forward_call(*args, **kwargs)
  File "/projappl/project_2009655/LMDrive/vision_encoder/timm/models/memfuser.py", line 830, in forward
    features, lidar_token = self.forward_features(
  File "/projappl/project_2009655/LMDrive/vision_encoder/timm/models/memfuser.py", line 801, in forward_features
    lidar_token = self.lidar_backbone(lidar, num_points) # Batchsize * embed_dim * 50 * 50
  File "/PUHTI_TYKKY_7d8FtWn/miniconda/envs/env1/lib/python3.8/site-packages/torch/nn/modules/module.py", line 1501, in _call_impl
    return forward_call(*args, **kwargs)
  File "/projappl/project_2009655/LMDrive/vision_encoder/timm/models/memfuser.py", line 500, in forward
    features = self.point_pillar_net(lidars, num_points)
  File "/PUHTI_TYKKY_7d8FtWn/miniconda/envs/env1/lib/python3.8/site-packages/torch/nn/modules/module.py", line 1501, in _call_impl
    return forward_call(*args, **kwargs)
  File "/projappl/project_2009655/LMDrive/vision_encoder/timm/models/pointpillar.py", line 145, in forward
    features = self.point_net(decorated_points, inverse_indices)
  File "/PUHTI_TYKKY_7d8FtWn/miniconda/envs/env1/lib/python3.8/site-packages/torch/nn/modules/module.py", line 1501, in _call_impl
    return forward_call(*args, **kwargs)
  File "/projappl/project_2009655/LMDrive/vision_encoder/timm/models/pointpillar.py", line 64, in forward
    feat_max = scatter_max(feat, inverse_indices, dim=0)[0]
  File "/PUHTI_TYKKY_7d8FtWn/miniconda/envs/env1/lib/python3.8/site-packages/torch_scatter/scatter.py", line 72, in scatter_max
    return torch.ops.torch_scatter.scatter_max(src, index, dim, out, dim_size)
  File "/PUHTI_TYKKY_7d8FtWn/miniconda/envs/env1/lib/python3.8/site-packages/torch/_ops.py", line 502, in __call__
    return self._op(*args, **kwargs or {})
RuntimeError: Not compiled with CUDA support
```

Be patient and look at the last trace ```torch.ops.torch_scatter.scatter_max```
it means we installed the torch-scatter without cuda support.
see the [soluton](https://github.com/rusty1s/pytorch_scatter/issues/271).

```Shell
pip install torch-scatter -f https://data.pyg.org/whl/torch-2.0.1+cu117.html
```


## My requirement.txt 
Here is the final python3.8 packages that I installed.
```Shell
accelerate==0.27.2
addict==2.4.0
altair==5.2.0
annotated-types==0.6.0
antlr4-python3-runtime==4.9.3
asttokens==2.4.1
attrs==23.2.0
backcall==0.2.0
bleach==6.1.0
blinker==1.7.0
blis==0.7.11
braceexpand==0.1.7
cachetools==5.3.3
catalogue==2.0.10
cfgv==3.4.0
chardet==5.1.0
charset-normalizer==3.3.2
click==8.1.7
cloudpathlib==0.16.0
cmake==3.28.3
comm==0.2.1
confection==0.1.4
ConfigArgParse==1.7
contexttimer==0.3.3
cycler==0.12.1
cymem==2.0.8
dash==2.16.1
dash-core-components==2.0.0
dash-html-components==2.0.0
dash-table==5.0.0
decorator==5.1.1
decord==0.6.0
dictor==0.1.12
diffusers==0.16.0
distlib==0.3.8
easydict==1.13
einops==0.7.0
elementpath==1.3.3
ephem==4.1.5
executing==2.0.1
fairscale==0.4.4
fastjsonschema==2.19.1
Flask==3.0.2
fonttools==4.49.0
fsspec==2024.2.0
ftfy==6.1.3
future==1.0.0
git-lfs==1.6
gitdb==4.0.11
GitPython==3.1.42
huggingface-hub==0.21.4
identify==2.5.35
idna==3.6
imageio==2.34.0
importlib_metadata==7.0.2
importlib_resources==6.1.3
iopath==0.1.10
ipython==8.12.3
ipywidgets==8.1.2
itsdangerous==2.1.2
jedi==0.19.1
joblib==1.3.2
jsonschema==4.21.1
jsonschema-specifications==2023.12.1
jupyter_core==5.7.1
jupyterlab_widgets==3.0.10
kaggle==1.6.6
kiwisolver==1.4.5
langcodes==3.3.0
lazy_loader==0.3
lit==17.0.6
markdown-it-py==3.0.0
MarkupSafe==2.1.5
matplotlib==3.5.3
matplotlib-inline==0.1.6
mdurl==0.1.2
mkl-service==2.4.0
murmurhash==1.0.10
identify==2.5.35
idna==3.6
imageio==2.34.0
importlib_metadata==7.0.2
importlib_resources==6.1.3
iopath==0.1.10
ipython==8.12.3
ipywidgets==8.1.2
itsdangerous==2.1.2
jedi==0.19.1
joblib==1.3.2
jsonschema==4.21.1
jsonschema-specifications==2023.12.1
jupyter_core==5.7.1
jupyterlab_widgets==3.0.10
kaggle==1.6.6
kiwisolver==1.4.5
langcodes==3.3.0
lazy_loader==0.3
lit==17.0.6
markdown-it-py==3.0.0
MarkupSafe==2.1.5
matplotlib==3.5.3
matplotlib-inline==0.1.6
mdurl==0.1.2
mkl-service==2.4.0
murmurhash==1.0.10
nbformat==5.9.2
nest-asyncio==1.6.0
nodeenv==1.8.0
nvidia-cublas-cu11==11.10.3.66
nvidia-cuda-cupti-cu11==11.7.101
nvidia-cuda-nvrtc-cu11==11.7.99
nvidia-cuda-runtime-cu11==11.7.99
nvidia-cudnn-cu11==8.5.0.96
nvidia-cufft-cu11==10.9.0.58
nvidia-curand-cu11==10.2.10.91
nvidia-cusolver-cu11==11.4.0.1
nvidia-cusparse-cu11==11.7.4.91
nvidia-nccl-cu11==2.14.3
nvidia-nvtx-cu11==11.7.91
omegaconf==2.3.0
open3d==0.18.0
opencv-contrib-python==4.5.5.64
opencv-python==4.5.5.64
opencv-python-headless==4.5.5.64
opendatasets==0.1.22
packaging==23.2
pandas==1.3.5
parso==0.8.3
peft==0.9.0
pexpect==4.9.0
pickleshare==0.7.5
pkgutil_resolve_name==1.3.10
platformdirs==4.2.0
plotly==5.19.0
portalocker==2.8.2
pre-commit==3.5.0
preshed==3.0.9
prompt-toolkit==3.0.43
protobuf==4.25.3
psutil==5.9.8
ptyprocess==0.7.0
pure-eval==0.2.2
py-trees==0.8.3
pyarrow==15.0.1
pycocoevalcap==1.2
pycocotools==2.0.7
pydantic==2.6.3
pydantic_core==2.16.3
pydeck==0.8.1b0
pydot==2.0.0
pygame==2.5.2
Pygments==2.17.2
pyparsing==3.1.2
pyquaternion==0.9.9
python-dateutil==2.9.0.post0
python-magic==0.4.27
python-slugify==8.0.4
pytz==2024.1
PyWavelets==1.4.1
PyYAML==6.0.1
referencing==0.33.0
regex==2023.12.25
retrying==1.3.4
rich==13.7.1
rpds-py==0.18.0
safetensors==0.4.2
scikit-image==0.21.0
transformers==4.28.0
triton==2.0.0
typer==0.9.0
typing_extensions==4.10.0
urllib3==2.2.1
virtualenv==20.25.1
wasabi==1.1.2
watchdog==4.0.0
wcwidth==0.2.13
weasel==0.3.4
webdataset==0.2.86
webencodings==0.5.1
Werkzeug==3.0.1
widgetsnbextension==4.0.10
xmlschema==1.0.18
zipp==3.17.0
```