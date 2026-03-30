# Getting Started

## Prerequisites

- Python 3.11+
- Google Cloud SDK (`gcloud`) — [install here](https://cloud.google.com/sdk/docs/install)
- A Google Cloud project with [billing enabled](https://docs.cloud.google.com/billing/docs/how-to/modify-project)

Authenticate with Google Cloud:

```bash
gcloud auth login
gcloud auth application-default login
```

Set your GCP project ID so the library knows where to run jobs:

```bash
export KINETIC_PROJECT="your-project-id"
```

Add this to your shell profile (`~/.bashrc`, `~/.zshrc`, etc.) to persist it. See :doc:`guides/configuration` for the full list of environment variables.

## Install

```bash
pip install keras-kinetic
```

This installs both the `@kinetic.run()` decorator and the `kinetic` CLI for managing infrastructure.

> **Note:** The [Pulumi](https://www.pulumi.com/) CLI (used for infrastructure
> provisioning) is bundled and managed automatically. It will be installed to
> `~/.kinetic/pulumi` on first use if not already present.

## Provision Infrastructure

If you are not the first Kinetic user in your org, you can skip this section.
Otherwise, run the one-time setup step to create a cluster for Kinetic:

```bash
kinetic up
```

This interactively prompts for your GCP project and accelerator type, then:

- Enables required APIs (Cloud Build, Artifact Registry, Cloud Storage, GKE)
- Creates an Artifact Registry repository for container images
- Provisions a GKE cluster with an accelerator node pool
- Configures Docker authentication and kubectl access

You can also run non-interactively:

```bash
kinetic up --project=my-project --accelerator=t4 --yes
```

> **Cleanup reminder:** When you're done, run `kinetic down` to tear down all resources and avoid ongoing charges. See [CLI Command here](cli.rst#kinetic-down).

## Run Your First Job

```python
import kinetic

@kinetic.run(accelerator="v5litepod-1")
def train_fashion_mnist():
    import keras
    import numpy as np

    # Load and preprocess the Fashion MNIST dataset
    (x_train, y_train), (x_test, y_test) = keras.datasets.fashion_mnist.load_data()
    x_train = x_train.astype("float32") / 255.0
    x_test = x_test.astype("float32") / 255.0
    x_train = np.expand_dims(x_train, -1)
    x_test = np.expand_dims(x_test, -1)

    # Build a simple convolutional model
    model = keras.Sequential([
        keras.layers.Input(shape=(28, 28, 1)),
        keras.layers.Conv2D(32, kernel_size=(3, 3), activation="relu"),
        keras.layers.MaxPooling2D(pool_size=(2, 2)),
        keras.layers.Flatten(),
        keras.layers.Dense(128, activation="relu"),
        keras.layers.Dense(10, activation="softmax"),
    ])

    model.compile(
        loss="sparse_categorical_crossentropy",
        optimizer="adam",
        metrics=["accuracy"],
    )

    # Train for a few epochs on the remote TPU
    model.fit(x_train, y_train, epochs=5, batch_size=64, validation_split=0.1)

    # Evaluate and return results
    score = model.evaluate(x_test, y_test, verbose=0)
    return f"Test loss: {score[0]:.4f}, Test accuracy: {score[1]:.4f}"

result = train_fashion_mnist()
print(result)
```

> **First run timing:** The initial execution takes longer (~5 minutes) because
> it builds a container image with your dependencies. Subsequent runs with
> unchanged dependencies use the cached image and start in less than a minute.
