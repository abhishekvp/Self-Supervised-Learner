<div align="center">

<img src="https://github.com/RudyVenguswamy/SpaceForceDataSearch/blob/main/curator.PNG" width="700px">


**The lightweight PyTorch wrapper for high-performance AI research.
Scale your models, not the boilerplate.**

---

<p align="center">
  <a href="https://www.pytorchlightning.ai/">Website</a> •
  <a href="#key-features">Key Features</a> •
  <a href="#how-to-use">How To Use</a> •
  <a href="https://pytorch-lightning.readthedocs.io/en/stable/">Docs</a> •
  <a href="#examples">Examples</a> •
  <a href="#community">Community</a> •
  <a href="#grid-ai">Grid AI</a> •
  <a href="#licence">Licence</a>
</p>

<!-- DO NOT ADD CONDA DOWNLOADS... README CHANGES MUST BE APPROVED BY EDEN OR WILL -->
[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/pytorch-lightning)](https://pypi.org/project/pytorch-lightning/)
[![PyPI Status](https://badge.fury.io/py/pytorch-lightning.svg)](https://badge.fury.io/py/pytorch-lightning)
[![PyPI Status](https://pepy.tech/badge/pytorch-lightning)](https://pepy.tech/project/pytorch-lightning)
[![Conda](https://img.shields.io/conda/v/conda-forge/pytorch-lightning?label=conda&color=success)](https://anaconda.org/conda-forge/pytorch-lightning)
[![DockerHub](https://img.shields.io/docker/pulls/pytorchlightning/pytorch_lightning.svg)](https://hub.docker.com/r/pytorchlightning/pytorch_lightning)
[![codecov](https://codecov.io/gh/PyTorchLightning/pytorch-lightning/branch/master/graph/badge.svg)](https://codecov.io/gh/PyTorchLightning/pytorch-lightning)

[![ReadTheDocs](https://readthedocs.org/projects/pytorch-lightning/badge/?version=stable)](https://pytorch-lightning.readthedocs.io/en/stable/)
[![Slack](https://img.shields.io/badge/slack-chat-green.svg?logo=slack)](https://join.slack.com/t/pytorch-lightning/shared_invite/zt-f6bl2l0l-JYMK3tbAgAmGRrlNr00f1A)
[![Discourse status](https://img.shields.io/discourse/status?server=https%3A%2F%2Fforums.pytorchlightning.ai)](https://forums.pytorchlightning.ai/)
[![license](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://github.com/PytorchLightning/pytorch-lightning/blob/master/LICENSE)
[![Next Release](https://img.shields.io/badge/Next%20Release-Nov%2021-<COLOR>.svg)](https://shields.io/)

<!--
[![CodeFactor](https://www.codefactor.io/repository/github/pytorchlightning/pytorch-lightning/badge)](https://www.codefactor.io/repository/github/pytorchlightning/pytorch-lightning)
-->
</div>

# Curator
A tool to quickly train a Self-Supervised Learning model and finetune the model on labeled data in order to help researchers rapidly curate a dataset for scientific studies without the need for machine learning, self supervised ML coding expertise.

__Requirements__: GPU with CUDA 10+ enabled, requirements.txt

Run `sh example.sh` to see the tool in action on the UC Merced land use dataset

## Using Your Own Data Set
```bash
SSL: python ssl_dali_distrib.py --ARGUMENTS
FT: python finetuner_dali_distrib.py --ARGUMENTS
```
To run it with your own data, please put your data in the following folder structure:
```
/Dataset
    /Class 1
        Image1.png
        Image2.png
    /Class 2
        Image3.png
        Image4.png

#When your dataset does not have labels yet you still need to nest it one level deep
/Dataset
    /Unlabelled
        Image1.png
        Image2.png

```

__Optional:__ To optimize your environment for deep learning, run this repo on the pytorch nvidia docker:

```bash
docker pull nvcr.io/nvidia/pytorch:20.12-py3
mkdir docker_folder
docker run --user=root -p 7000-8000:7000-8000/tcp --volume="/etc/group:/etc/group:ro" --volume="/etc/passwd:/etc/passwd:ro" --volume="/etc/shadow:/etc/shadow:ro" --volume="/etc/sudoers.d:/etc/sudoers.d:ro" --gpus all -it --rm -v /home/rudyvenguswamy/docker_folder:/inside_docker nvcr.io/nvidia/pytorch:20.12-py3
apt update
apt install -y libgl1-mesa-glx
#now clone repo inside container, install requirements as usual, login to wandb if you'd like to
```

## How to access models after training in python environment
Both self-supervised models and finetuned models can be accessed and used normally as `pl_bolts.LightningModule` models. They function the same as a pytorch nn.Module but have added functionality that works with a pytorch lightning Trainer.

For example:
```python
from SpaceForceDataSearch.ssl_dali_distrib import SIMCLR 
from SpaceForceDataSearch.finetuner_dali_distrib import finetuner

simclr_model = SIMCLR.load_from_checkpoint('PATH_TO_SSL_MODEL.ckpt')
finetuned_model = finetuner.load_from_checkpoint('PATH_TO_SSL_MODEL.ckpt')

print(simclr_model.encoder)
print(finetuned_model.encoder)

datapoint = my_load_and_transform_function('example.jpg')

embedding = simclr_model(datapoint)
prediction = finetuned_model(datapoint)
```

## Using Your Own Encoder
If you don't want to use the predefined encoders in encoders_dali.py, it's very easy to modify the code. To add your own model:
1) Modify encoders_dali.py and add your custom model, which needs to be of type torch.nn.Module.
2) Modify the load_encoder method of encoders_dali.py to have your model as an option. You will need to specify the output size (vector length - int) of the embedding generated for a single datapoint as well. Add another elif statement like so to do so:
```
 elif encoder_name == 'my_custom_encoder':
        model, embedding_size = my_custom_encoder(my_init_params), my_model_output_embedding_size: int
```

## Coming Updates
- Cluster Visualizations for Embeddings 
- Supporting numpy, TFDS datasets
- Saliency Maps for Embeddings


