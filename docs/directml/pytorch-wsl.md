---
title: Enable PyTorch with DirectML on WSL 2
description: Instructions for running PyTorch inferencing on your existing hardware with **PyTorch with DirectML**, using WSL.
ms.topic: article
ms.date: 05/21/2024
---

# Enable PyTorch with DirectML on WSL 2

PyTorch with DirectML provides an easy-to-use way for developers to try out the latest and greatest AI models on their Windows machine. You can download PyTorch with DirectML by installing the [**torch-directml**](https://pypi.org/project/torch-directml/) PyPi package. Once set up, you can start with our [samples](https://github.com/microsoft/DirectML/tree/master/PyTorch) or use the AI Toolkit for VS Code.

## Check your version of Windows 

The **torch-directml** package in the Windows Subsystem for Linux (WSL) 2 works starting with Windows 11 (Build 22000 or higher). You can check your build version number by running `winver` via the **Run** command (Windows logo key + R).

## Check for GPU driver updates
Ensure you have the latest GPU driver installed. Select **Check for updates** in the **Windows Update** section of the **Settings** app.

## Set up Torch-DirectML

### Install WSL 2

To install the Windows Subsystem for Linux (WSL) 2, see the instructions in [Install WSL](/windows/wsl/install).

Then install the WSL GUI driver by following the instructions in the `README.md` file in the [microsoft/wslg](https://github.com/microsoft/wslg) GitHub repository.

### Set up a Python environment 

We recommend that you set up a virtual Python environment inside WSL 2. There are many tools that you can use to set up a virtual Python environment&mdash;in this topic we'll use Anaconda's [Miniconda](https://docs.anaconda.com/free/miniconda/). The rest of this setup assumes that you use a Miniconda environment.

Install Miniconda by following the [Linux installer guidance](https://docs.anaconda.com/free/miniconda/miniconda-install/) on Anaconda's site, or by running the following commands in WSL 2.

```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh 
bash Miniconda3-latest-Linux-x86_64.sh
```

Once Miniconda is installed, create a Python environment named **pytdml**, and activate it through the following commands:

```
conda create --name pytdml -y
conda activate pytdml
```

### Install PyTorch and Torch-DirectML

> [!NOTE]
> The **torch-directml** package supports up to PyTorch 2.2.

All that is needed to get setup is installing the latest release of **torch-directml** by running the following command:

```
pip install torch-directml
```

### Verification and Device Creation

Once you've installed the **torch-directml** package, you can verify that it runs correctly by adding two tensors. First start an interactive Python session, and import Torch with the following lines:

```
import torch
import torch_directml
dml = torch_directml.device()
```

The current release of **torch-directml** is mapped to the "PrivateUse1" Torch backend. The torch_directml.device() API is a convenient wrapper for sending your tensors to the DirectML device.

With the DirectML device created, you can now define two simple tensors; one tensor containing a 1 and another containing a 2. Place the tensors on the "dml" device.

```
tensor1 = torch.tensor([1]).to(dml) # Note that dml is a variable, not a string!
tensor2 = torch.tensor([2]).to(dml)
```

Add the tensors together, and print the results.

```
dml_algebra = tensor1 + tensor2
dml_algebra.item()
```

You should see the number 3 being output, as in the example below.

```
>>> import torch
>>> tensor1 = torch.tensor([1]).to(dml)
>>> tensor2 = torch.tensor([2]).to(dml)
>>> dml_algebra = tensor1 + tensor2
>>> dml_algebra.item()
3
```  

## PyTorch with DirectML samples and feedback 

Check out [our samples](https://github.com/microsoft/DirectML/tree/master/PyTorch) to see more uses of PyTorch with DirectML. If you run into issues, or have feedback on the PyTorch with DirectML package, then please [connect with our team here](https://github.com/microsoft/DirectML/issues).