<div align="center">
  <a href="https://autooptm.com"><img src=".autooptm/logo.png" width="96" alt="AutoOptm"></a>

  <h1>vision · optimized by <a href="https://autooptm.com">AutoOptm</a></h1>

  <p><b>3.44x faster end to end</b> on the command below, output verified against the stock program.</p>

  <p>
    <a href="https://autooptm.com"><img alt="speedup" src="https://img.shields.io/badge/end--to--end-3.44x-2ea44f"></a>
    <a href="https://github.com/pytorch/vision/commit/447c9374be54faf736439d909b99703f0d5bd4ff"><img alt="base" src="https://img.shields.io/badge/upstream-447c9374be54-blue"></a>
    <img alt="card" src="https://img.shields.io/badge/measured%20on-RTX%204090-lightgrey">
  </p>
</div>

> This is a fork of [pytorch/vision](https://github.com/pytorch/vision) at commit
> [`447c9374be54`](https://github.com/pytorch/vision/commit/447c9374be54faf736439d909b99703f0d5bd4ff) with the AutoOptm patch applied on top.
> The optimisation was found, measured and verified automatically by [AutoOptm](https://autooptm.com);
> the patch is also kept at [`.autooptm/autooptm.patch`](.autooptm/autooptm.patch).

## The result

| | |
|---|---|
| **Command** | `python train.py --model resnet50 --fast` |
| **Entry point** | `references/classification/train.py` |
| **Unit measured** | one training step of ResNet-50 at the reference recipe's batch size (end to end) |
| **Before (stock)** | 0.02293 s per unit |
| **After (this tree, all switches default ON)** | 0.006424 s per unit |
| **Speedup** | **3.44x** end to end, noise floor of the host 2.25% |
| **Output** | verified against the frozen stock reference on the pinned inputs and on a held-out set the optimiser never saw |

### What changed

| File | Where | Gain (alone) |
|---|---|---|
| `references/classification/train.py` | train_one_epoch (new helper) | 1.16x |
| `references/classification/train.py` | main() -> torch.optim.SGD | 1.1x |
| `references/classification/train.py` | train_one_epoch | 1.22x |
| `references/classification/train.py` | main() model + train_one_epoch + evaluate | 1.01x |
| `references/classification/train.py` | _flush_metrics / train_one_epoch | 1.02x |
| `references/classification/presets.py` | ClassificationPresetTrain.__init__ | 3.57x |
| `references/classification/train.py` | train_one_epoch (new helper) | 3.57x |
| `references/classification/train.py` | main() DataLoader construction | 3.57x |
| `references/classification/train.py` | train_one_epoch | 3.57x |
| `references/classification/train.py` | write_checkpoint / main() epoch loop | 3.57x |
| `references/classification/train.py` | write_checkpoint / _clone_state / join_checkpoint | 3.57x |
| `references/classification/train.py` | get_args_parser | 1x |

## Reproduce

```bash
git clone https://github.com/autooptm/vision-ao.git
cd vision-ao
# set up exactly as upstream documents, then:
python train.py --model resnet50 --fast
```

The diff against upstream is one commit: `git log -1 -p` shows it, and
`git diff 447c9374be54` is the same patch as `.autooptm/autooptm.patch`.

---

<div align="center"><sub>Optimized by <a href="https://autooptm.com">AutoOptm</a> — point it at a repository, get back a verified speedup and the patch.</sub></div>

---

# torchvision

[![total torchvision downloads](https://pepy.tech/badge/torchvision)](https://pepy.tech/project/torchvision)
[![documentation](https://img.shields.io/badge/dynamic/json.svg?label=docs&url=https%3A%2F%2Fpypi.org%2Fpypi%2Ftorchvision%2Fjson&query=%24.info.version&colorB=brightgreen&prefix=v)](https://pytorch.org/vision/stable/index.html)

The torchvision package consists of popular datasets, model architectures, and common image transformations for computer
vision.

## Installation

Please refer to the [official
instructions](https://pytorch.org/get-started/locally/) to install the stable
versions of `torch` and `torchvision` on your system.

To build source, refer to our [contributing
page](https://github.com/pytorch/vision/blob/main/CONTRIBUTING.md#development-installation).

The following is the corresponding `torchvision` versions and supported Python
versions.

| `torch`            | `torchvision`      | Python              |
| ------------------ | ------------------ | ------------------- |
| `main` / `nightly` | `main` / `nightly` | `>=3.10`, `<=3.14`  |
| `2.13`             | `0.28`             | `>=3.10`, `<=3.14`  |
| `2.12`             | `0.27`             | `>=3.10`, `<=3.14`  |
| `2.11`             | `0.26`             | `>=3.10`, `<=3.14`  |
| `2.10`             | `0.25`             | `>=3.10`, `<=3.14`  |


<details>
    <summary>older versions</summary>

| `torch` | `torchvision`     | Python                    |
|---------|-------------------|---------------------------|
| `2.9`              | `0.24`             | `>=3.10`, `<=3.14`  |
| `2.8`              | `0.23`             | `>=3.9`, `<=3.13`   |
| `2.7`              | `0.22`             | `>=3.9`, `<=3.13`   |
| `2.6`              | `0.21`             | `>=3.9`, `<=3.12`   |
| `2.5`              | `0.20`             | `>=3.9`, `<=3.12`   |
| `2.4`              | `0.19`             | `>=3.8`, `<=3.12`   |
| `2.3`              | `0.18`             | `>=3.8`, `<=3.12`   |
| `2.2`              | `0.17`             | `>=3.8`, `<=3.11`   |
| `2.1`              | `0.16`             | `>=3.8`, `<=3.11`   |
| `2.0`              | `0.15`             | `>=3.8`, `<=3.11`   |
| `1.13`  | `0.14`            | `>=3.7.2`, `<=3.10`       |
| `1.12`  | `0.13`            | `>=3.7`, `<=3.10`         |
| `1.11`  | `0.12`            | `>=3.7`, `<=3.10`         |
| `1.10`  | `0.11`            | `>=3.6`, `<=3.9`          |
| `1.9`   | `0.10`            | `>=3.6`, `<=3.9`          |
| `1.8`   | `0.9`             | `>=3.6`, `<=3.9`          |
| `1.7`   | `0.8`             | `>=3.6`, `<=3.9`          |
| `1.6`   | `0.7`             | `>=3.6`, `<=3.8`          |
| `1.5`   | `0.6`             | `>=3.5`, `<=3.8`          |
| `1.4`   | `0.5`             | `==2.7`, `>=3.5`, `<=3.8` |
| `1.3`   | `0.4.2` / `0.4.3` | `==2.7`, `>=3.5`, `<=3.7` |
| `1.2`   | `0.4.1`           | `==2.7`, `>=3.5`, `<=3.7` |
| `1.1`   | `0.3`             | `==2.7`, `>=3.5`, `<=3.7` |
| `<=1.0` | `0.2`             | `==2.7`, `>=3.5`, `<=3.7` |

</details>

## Image Backends

Torchvision currently supports the following image backends:

- torch tensors
- PIL images:
    - [Pillow](https://python-pillow.org/)
    - [Pillow-SIMD](https://github.com/uploadcare/pillow-simd) - a **much faster** drop-in replacement for Pillow with SIMD.

Read more in in our [docs](https://pytorch.org/vision/stable/transforms.html).

## Documentation

You can find the API documentation on the pytorch website: <https://pytorch.org/vision/stable/index.html>

## Contributing

See the [CONTRIBUTING](CONTRIBUTING.md) file for how to help out.

## Disclaimer on Datasets

This is a utility library that downloads and prepares public datasets. We do not host or distribute these datasets,
vouch for their quality or fairness, or claim that you have license to use the dataset. It is your responsibility to
determine whether you have permission to use the dataset under the dataset's license.

If you're a dataset owner and wish to update any part of it (description, citation, etc.), or do not want your dataset
to be included in this library, please get in touch through a GitHub issue. Thanks for your contribution to the ML
community!

## Pre-trained Model License

The pre-trained models provided in this library may have their own licenses or terms and conditions derived from the
dataset used for training. It is your responsibility to determine whether you have permission to use the models for your
use case.

More specifically, SWAG models are released under the CC-BY-NC 4.0 license. See
[SWAG LICENSE](https://github.com/facebookresearch/SWAG/blob/main/LICENSE) for additional details.

## Citing TorchVision

If you find TorchVision useful in your work, please consider citing the following BibTeX entry:

```bibtex
@software{torchvision2016,
    title        = {TorchVision: PyTorch's Computer Vision library},
    author       = {TorchVision maintainers and contributors},
    year         = 2016,
    journal      = {GitHub repository},
    publisher    = {GitHub},
    howpublished = {\url{https://github.com/pytorch/vision}}
}
```
