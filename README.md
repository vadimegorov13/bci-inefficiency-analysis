## Usage

### 1. Create a virtual environment

From the repository root:

```bash
python3.11 -m venv .venv
source .venv/bin/activate
```

### 2. Install dependencies

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### 3. Register the environment as a notebook kernel

```bash
python -m ipykernel install --user --name bci-illiteracy --display-name "Python (bci-illiteracy)"
```

After that, select **Python (bci-illiteracy)** as the kernel in Jupyter or VS Code.

### 4. Run the notebooks

Open the project notebooks and run them with the registered kernel.
