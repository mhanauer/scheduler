# scheduler

# Azure ML Pipeline Scheduler with Python

This repository contains a Python script that demonstrates how to set up an Azure ML Pipeline, publish it, and schedule it to run at regular intervals. The script leverages the Azure Machine Learning Python SDK to interact with Azure ML Workspace and associated resources.

## Table of Contents

1. [Requirements](#requirements)
2. [Getting Started](#getting-started)
3. [Understanding the Code](#understanding-the-code)
4. [Running the Script](#running-the-script)

---

## Requirements

- Azure subscription
- Azure ML Workspace
- Azure Compute Target with the name `matthewhanauer3`
- Python 3.6 or higher
- Install the required Python packages mentioned in `requirements.txt`

---

## Getting Started

1. Clone this repository to your local machine.

    ```bash
    git clone https://github.com/your-repo/azure-ml-pipeline-scheduler.git
    ```

2. Navigate to the directory and install the required Python packages.

    ```bash
    cd azure-ml-pipeline-scheduler
    pip install -r requirements.txt
    ```

---

## Understanding the Code

The script performs the following actions:

- **Initialize Azure ML Workspace**: The script starts by initializing the Azure ML Workspace using the `Workspace.from_config()` method.
  
    ```python
    ws = Workspace.from_config()
    ```

- **Create Python Environment**: A Python environment is created from a `requirements.txt` file. This environment is used for the pipeline run.
  
    ```python
    python_env = Environment.from_pip_requirements("pipeline_env", "requirements.txt")
    ```

- **Compute Target**: The script specifies an existing Azure Compute Target named `matthewhanauer3`. Make sure this compute target exists in your Azure ML Workspace.
  
    ```python
    compute_target = ComputeTarget(workspace=ws, name=existing_compute_target_name)
    ```

- **Python Script Step**: Specifies the Python script (`data_modeling_ml.py`) to run in the pipeline.

    ```python
    script_step = PythonScriptStep(
        name="Run Scheduled Script",
        script_name="data_modeling_ml.py",
        source_directory="./",
        runconfig=pipeline_run_config,
        compute_target=compute_target,
        allow_reuse=False,
    )
    ```

- **Publish Pipeline**: The pipeline is published to be accessible later for scheduling.
  
    ```python
    published_pipeline = pipeline.publish(
        name="My_Published_Pipeline",
        description="Published pipeline to run the script every minute.",
    )
    ```

- **Schedule the Pipeline**: The published pipeline is scheduled to run every minute.

    ```python
    recurrence = ScheduleRecurrence(frequency="Minute", interval=1)
    schedule = Schedule.create(
        ws,
        name="My_Schedule",
        description="Runs pipeline every minute",
        pipeline_id=pipeline_id,
        experiment_name=experiment_name,
        recurrence=recurrence,
    )
    ```

---

## Running the Script

Execute the script to initialize the Azure resources, publish the pipeline, and schedule it to run every minute.

```bash
python main.py
