# Kickstarting Databricks Asset Bundles: Streamlining Your Workflow

*By Maximiliano Fonseca · 2025 

In my previous posts,  [“Simplifying Python Project Initialization”](https://medium.com/@maxfonseca.r/simplifying-python-project-initialization-e9c933c512ce) and [“Kickstarting Python Projects: A Personal Guide to Efficiency”](https://medium.com/@maxfonseca.r/kickstarting-python-projects-a-personal-guide-to-efficiency-758371ec18a9) , I shared how I optimized Python project setups using Poetry and custom initialization scripts. Building on those best practices, this time we’ll dive into Databricks Asset Bundles — a robust way to organize, deploy, and manage Databricks notebooks and resources efficiently.

---

## What are Databricks Asset Bundles?

Databricks Asset Bundles are structured collections of Databricks resources (notebooks, pipelines, configuration files) packed together to ensure consistent deployment across environments. Think of them as the next evolution of your Python project structures, but tailored for Databricks.  
Official docs: https://docs.databricks.com/dev-tools/bundles/index.html

---

## Before Starting

1. **PyEnv** installed.  
   Official docs: https://github.com/pyenv/pyenv  
2. **VS Code** installed.  
   Official docs: https://code.visualstudio.com/

---

## Step 1: Install Databricks CLI

We’ll use the standalone Databricks CLI (v0.205+). Install it on macOS/Linux as follows:

```bash
curl -fsSL https://raw.githubusercontent.com/databricks/setup-cli/main/install.sh | sh
```
Windows:

```bash
winget search databricks
winget install Databricks.DatabricksCLI
```


Verify the installation:

```bash
databricks version
```

Configure authentication:

```bash
databricks auth login
# Enter your profile name (e.g. "dev") and your workspace URL when prompted
```

---

## Step 2: Install Databricks VS Code Extension

Streamline development by integrating Databricks into VS Code:

1. Open VS Code.  
2. Go to **Extensions** (Ctrl+Shift+X).  
3. Search for **Databricks** and install the official extension.  
   Marketplace: https://marketplace.visualstudio.com/items?itemName=databricks.databricks

This brings notebook browsing, execution, and bundle commands right into your editor.

---

## Step 3: Initialize a Databricks Asset Bundle

In your terminal, navigate to where you want to create your bundle (e.g., a `databricks/` folder) and run:

```bash
# Use -p <profile_name> if you have multiple profiles
databricks bundle init -p dev
```

Choose the **default-python** template and answer the prompts:

```
Unique name for this project [my_project]: test
Include a stub (sample) notebook in 'test/src': yes
Include a stub (sample) Delta Live Tables pipeline in 'test/src': no
Include a stub (sample) Python package in 'test/src': no
```

Your new bundle is created in `test/`. Structure:

```
test/
├── databricks.yml      # Bundle configuration (profile, workspace, etc.)
├── README.md           # Getting started instructions
├── fixtures/           # Sample datasets/test fixtures
├── resources/          # YAML defs for Databricks assets (jobs, pipelines, etc.)
│   └── scratch/        # Sandbox for temp/experimental notebooks & scripts
└── src/                # Your project code
    └── notebooks.ipynb # Auto-generated sample notebook
```

---

## Step 4: Structuring Your Project

Mirror your Python project layout, but for Databricks:

```bash
mkdir -p src/config src/utils src/notebooks
touch src/config/config.py src/utils/__init__.py src/notebooks/notebook.bronze.ipynb
```

```
your-databricks-project/
├── databricks.yml
└── src/
    ├── notebooks/
    │   └── notebook.bronze.ipynb
    ├── config/
    │   └── config.py
    └── utils/
        └── helpers.py
```

### Folder Advice

- **src/config/**: Parameter files, credential templates, environment overrides.  
- **src/utils/**: Common functions (data loaders, transformations).  
- **src/notebooks/**: Analysis or pipeline notebooks importing from `config` & `utils`.

---

## Step 5: Configure the Databricks Extension in VS Code

1. From your project root:  
   ```bash
   code .
   ```  
2. Click the Databricks icon in the Activity Bar.  
3. Select your `dev` profile under **Target**.  
4. Ensure **Auth Type** shows `Profile 'dev'`.  
5. (Optional) Choose **Serverless** for faster cluster startup.

### Python Environment

![image](https://github.com/user-attachments/assets/a89e8fe9-0faa-4304-a193-b3e210e9c683)

Version needs to match in your local and cluster selected

- Install/manage Python with **pyenv**:
  ```bash
  pyenv install 3.11.12
  pyenv local 3.11.12
  ```
- Create & activate a venv:
  ```bash
  python -m venv .venv
  source .venv/bin/activate
  ```
- In VS Code, switch the interpreter (bottom-left) to `.venv` or the pyenv version.

When opening a notebook (`.ipynb`), agree to install the Databricks-built-in kernel for full execution & autocomplete support.

---

## Final Thoughts

Just as Poetry and init scripts leveled up my Python workflows, Databricks Asset Bundles will do the same for your Databricks projects — bringing consistency, reproducibility, and ease of deployment. Give it a try in your next project and let me know how it goes!

---

*Tags: [Databricks] · [Databricks Asset Bundles] · [CLI] · [Azure Databricks]*
