# What is CaraML?

CaraML is a **Machine Learning Operations (MLOps) platform** that help data science teams to increase development velocity and make production operations as simple as possible.

## **Main components for CaraML:**

### **Models**

Merlin is a framework for serving machine learning model. The project was born of the belief that model deployment should be:

* Easy and self-serve: Human should not become the bottleneck for deploying model into production.
* Scalable: The model deployed should be able to handle Gojek scale and beyond.
* Fast: The framework should be able to let user iterate quickly.
* Cost Efficient: It should provide all benefit above in a cost efficient manner.

Merlin attempt to do so by:

* **Abstracting Infrastructure** Merlin uses familiar concept such as Project, Model, and Version as its core component and abstract away complexity of deploying service from user.
* **Auto Scaling** Merlin is built on top KNative and KFServing to provide a production ready serverless solution.

### **Feature Store**

Feature Store is an operational data system for managing and serving machine learning features to models in production. CaraML's Feature store is powered by [Feast](https://feast.dev/).&#x20;

### **Routers**

Turing is a fast, scalable and extensible system that can be used to design, deploy and evaluate ML experiments in production. Broadly, its capabilities can be divided into the following two areas that may be utilised in conjunction or separately:

* **Experimentation** - Turing supports designing and managing experiment configurations and running them, through its in-built experiment engine.
* **Orchestration** - Turing supports deploying experiment workflows (through composable 'routers'). It is designed to work with pluggable pre- and post-processors and is backed by existing systems like Merlin for model endpoints. Turing takes care of all of the core Engineering aspects such as traffic routing, autoscaling, outcome logging, system monitoring and alerting.

### **Experiments**

Turing supports designing and managing experiment configurations in a safe and holistic manner. At run time, these configurations can be used (within the Turing router, or externally) to run the experiments and generate treatments. The experiments can be run either deterministically (A/B Experiments) or as a function of time (Switchback Experiments), or a combination of both (Randomized Switchbacks).

### **Pipelines**

Data Aware Pipeline (DAP) is a set of solutions to build data application systems like ETL processes and ML pipelines. DAP is powered by [Flyte](https://docs.flyte.org/en/latest/), an open-source workflow automation platform to create concurrent, scalable, and maintainable workflows for machine learning and data processing.

****

### Guides: Jump right in

Follow our handy guides to get started on the basics as quickly as possible:

{% content-ref url="user-guides/projects/" %}
[projects](user-guides/projects/)
{% endcontent-ref %}

{% content-ref url="user-guides/models/" %}
[models](user-guides/models/)
{% endcontent-ref %}

{% content-ref url="user-guides/feature-store/" %}
[feature-store](user-guides/feature-store/)
{% endcontent-ref %}

{% content-ref url="user-guides/routers/" %}
[routers](user-guides/routers/)
{% endcontent-ref %}

{% content-ref url="user-guides/experiments/" %}
[experiments](user-guides/experiments/)
{% endcontent-ref %}

{% content-ref url="user-guides/pipelines/" %}
[pipelines](user-guides/pipelines/)
{% endcontent-ref %}



## Deploying CaraML in your infrastructure?

Please refer to our deployment guide below to deploy CaraML into your own infrastructure

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}
