![GCP_Professional_Machine_Learning_Engineer_logo](../../images/GCP-Professional-Machine-Learning-Engineer.png)

# Google Cloud Certified - Professional Machine Learning Engineer

## Machine Learning Concept

- A way to derive **repeated predicted insights from data**
- Stages insist of: Training and Inference (Prediction)
- Considered to provide *scalable*, *automatic* and *personalized* solutions

## Framing a business problem as a machine learning problem

1. cast it as a machine learning problem

- [x] what is being predicted?
- [x] what data is needed?

2. cast it as a software problem

- [x] what is the API for the problem during prediction?
- [x] who will use the prediction service?
- [x] How are they doing it today?

3. cast it in the framework of a data problem

- [x] what are some key actions to collect, analyze, predict and react to the data or predictions?


## vertex Notebooks

- managed notebooks

    :white_check_mark: control hardware (GPUs, RAM)

    :white_check_mark: custom containers (add custom container images)

    :white_check_mark: use Cloud Storage and BigQuery extension to browse data

    :white_check_mark: Dataproc integration

- user-managed notebooks

    Ideal for:
    * heavily customized Deep Learning VM 
    * health status monitoring
    * have specific network and security needs (set VPC service control)

Both notebooks are protected by Google Cloud authentication and authorization

Both notebooks support GPU accelerators and sync with Github repository

Both notebooks are pre-packaged with JupyterLab and have a pre-installed suite of deep learning packages, including support for the TensorFlow and PyTorch frameworks