<div align=center><a href="https://www.intel.ai/plaidml"><img
src="docs/images/plaid-final.png" height="200"></a><br>

*A platform for making deep learning work everywhere.*

[Documentation](https://vertexai-plaidml.readthedocs-hosted.com/) |
[Installation Instructions](docs/install.md) |
[Building PlaidML](docs/building.md) |
[Contributing](docs/contributing.rst) |
[Troubleshooting](docs/troubleshooting.md) |
[Reporting Issues](#reporting-issues)

</div>

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://github.com/plaidml/plaidml/blob/master/LICENSE)
[![Build status](https://badge.buildkite.com/5c9add6b89a14fd498e69a5035062368480e688c4c74cbfab3.svg?branch=master)](https://buildkite.com/plaidml/plaidml-plaidml)

PlaidML is an advanced and portable tensor compiler for enabling deep learning
on laptops, embedded devices, or other devices where the available computing
hardware is not well supported or the available software stack contains
unpalatable license restrictions.

PlaidML sits underneath common machine learning frameworks, enabling users to
access any hardware supported by PlaidML. PlaidML supports [Keras], [ONNX], and
[nGraph].

As a component within the [nGraph Compiler stack], PlaidML further extends the
capabilities of specialized deep-learning hardware (especially GPUs,) and makes
it both easier and faster to access or make use of subgraph-level optimizations
that would otherwise be bounded by the compute limitations of the device.

As a component under [Keras], PlaidML can accelerate training workloads with
customized or automatically-generated Tile code. It works especially well on
GPUs, and it doesn't require use of CUDA/cuDNN on Nvidia* hardware, while
achieving comparable performance.

PlaidML works on all major operating systems: Linux, macOS, and Windows.

If you are using a hardware target not supported by PlaidML by default, such as
Clover, check out the instructions at [building PlaidML](docs/building.md) to
build a custom configuration to support your hardware.

## Prerequisites
- Python (v2 supported, v3 recommended)
- OpenCL 1.2 or greater

## Quick Start

See the [troubleshooting documentation](docs/troubleshooting.md) for common
issues.

```
virtualenv plaidml
source plaidml/bin/activate
pip install plaidml-keras plaidbench
```

Choose which accelerator you'd like to use (many computers, especially laptops,
have multiple):

```
plaidml-setup
```

Next, try benchmarking MobileNet inference performance:

```
plaidbench keras mobilenet
```

Or, try training MobileNet:

```
plaidbench --batch-size 16 keras --train mobilenet
```

## Installation Instructions

We support a variety of operating systems and installation methods.

* [Ubuntu Linux](docs/install.md#ubuntu-linux)
* [macOS](docs/install.md#macos)
* [Windows](docs/install.md#windows)

## Demos and Related Projects

### Plaidbench

[Plaidbench](https://github.com/plaidml/plaidbench) is a performance testing
suite designed to help users compare the performance of different cards and
different frameworks.

### Hello VGG

One of the great things about Keras is how easy it is to play with state of the
art networks. Here's all the code you need to run VGG-19:

```python
#!/usr/bin/env python
import numpy as np
import os
import time

os.environ["KERAS_BACKEND"] = "plaidml.keras.backend"

import keras
import keras.applications as kapp
from keras.datasets import cifar10

(x_train, y_train_cats), (x_test, y_test_cats) = cifar10.load_data()
batch_size = 8
x_train = x_train[:batch_size]
x_train = np.repeat(np.repeat(x_train, 7, axis=1), 7, axis=2)
model = kapp.VGG19()
model.compile(optimizer='sgd', loss='categorical_crossentropy',
              metrics=['accuracy'])

print("Running initial batch (compiling tile program)")
y = model.predict(x=x_train, batch_size=batch_size)

# Now start the clock and run 10 batches
print("Timing inference...")
start = time.time()
for i in range(10):
    y = model.predict(x=x_train, batch_size=batch_size)
print("Ran in {} seconds".format(time.time() - start))

```

## Reporting Issues

Either open a ticket on [GitHub] or join our [slack workspace (#plaidml)].

## CI & Validation

### Validated Hardware

A comprehensive set of tests for each release are run against the hardware
targets listed below.

* AMD
  * R9 Nano
  * RX 480
  * Vega 10


* Intel
  * HD4000
  * HD Graphics 505


* NVIDIA
  * K80
  * GT 640M
  * GTX 1050
  * GTX 1070

### Validated Networks

We support all of the Keras application networks from
current versions of 2.x. Validated networks are tested for performance and
correctness as part of our continuous integration system.

* CNNs
  * Inception v3
  * ResNet50
  * VGG19
  * Xception
  * MobileNet
  * DenseNet
  * ShuffleNet


* LSTM
  * examples/imdb_lstm.py (from keras)

[nGraph Compiler stack]: https://ngraph.nervanasys.com/docs/latest/
[Keras]: https://keras.io/
[GitHub]: https://github.com/plaidml/plaidml/issues
[plaidml-dev]: https://groups.google.com/forum/#!forum/plaidml-dev
[Onnx]: https://github.com/onnx
[nGraph]: https://github.com/NervanaSystems/ngraph
[slack workspace (#plaidml)]: https://join.slack.com/t/ngraph/shared_invite/enQtNjY1Njk4OTczMzEyLWIyZjZkMDNiNzJlYWQ3MGIyZTg2NjRkODAyYWZlZWY5MmRiODdlNzVkMjcxNjNmNWEyZjNkMDVhMTgwY2IzOWQ
