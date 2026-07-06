# fcolab 🚀

`fcolab` is an elegant, context-manager-based Python wrapper for `google-colab-cli`. 
It allows you to seamlessly spin up remote Google Colab GPUs, execute Python scripts headlessly, transfer files, and automatically terminate the session to prevent billing—all entirely within your local Python code.

## Installation

```bash
pip install .
```

(Ensure you have authenticated your `google-colab-cli` credentials first via `colab login`).

## Usage

Instead of manually orchestrating `subprocess` calls to the `colab` CLI and worrying about orphaned instances when your script crashes, `fcolab` handles the lifecycle for you.

```python
from fcolab import FColab

# Start a Colab session (automatically terminates when the block ends or an exception occurs)
with FColab(session_name="my_worker", gpu="T4") as colab:
    
    # 1. Upload local data to the Colab instance
    colab.upload("local_dataset.zip", "/content/dataset.zip")
    
    # 2. Execute a local python script remotely on the Colab instance GPU
    colab.exec_script("train_model.py")
    
    # 3. Download the resulting artifacts back to your local machine
    colab.download("/content/model_weights.pt", "./model_weights.pt")
    
# The session is automatically stopped here! No stray bills.
```

## Methods

- `__init__(session_name="default", gpu="T4")`: Initializes the wrapper. `gpu` can be `T4`, `V100`, `A100`, etc., based on your Colab subscription.
- `start()`: Manually starts the session. (Implicitly called if using `with`).
- `stop()`: Manually terminates the session. (Implicitly called if using `with`).
- `upload(local_path, remote_path)`: Pushes a file to Colab.
- `download(remote_path, local_path)`: Pulls a file from Colab.
- `exec_script(local_script_path)`: Uploads and runs a local python script.
- `execute_remote(command)`: Executes a shell command on the Colab machine.

## License
MIT