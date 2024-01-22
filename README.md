# What is CaraML?

CaraML is a **Machine Learning Operations (MLOps) platform** that helps data scientists focus on data science by abstracting the engineering complexities of building and running large scale Machine Learning applications.

To access this documentation in GitBook, please follow this URL [CaraML Docs](https://caraml.gitbook.io/caraml-doc/)

## **Main components for CaraML:**

### **Models**

Models component is a framework for serving machine learning models. The project was born of the belief that model deployment should be:

* Easy and self-serve: Humans should not become the bottleneck for deploying models into production.
* Scalable: Any model deployed should be able to handle large scale traffic
* Fast: The framework should be able to let users iterate quickly.
* Cost Efficient: It should provide all benefits above in a cost efficient manner.

CaraML Models solves this by:

* **Abstracting Infrastructure:** Models uses familiar concepts such as Project, Model, and Version as its core component and abstracts away complexity of deploying services from our users.
* **Auto Scaling:** Models component is built on top KNative and KFServing to provide a production ready serverless solution which can automatically scale up and down to handle dynamic demand.

CaraML Models project code name is Merlin, which may show in code, SDK, API documentations

### **Feature Store**

CaraML Feature Store is an operational data system for managing and serving machine learning features to models in production. CaraML's Feature store is forked from the open source feature store [Feast](https://feast.dev/), and customised to be geared towards more production ready use cases.

### **Routers**

CaraML routers is a fast, scalable and extensible system that can be used to design, deploy and evaluate ML experiments in production. Broadly, its capabilities can be divided into the following two areas that may be utilised in conjunction or separately:

* **Experimentation** - Routers component supports designing and managing experiment configurations and running them, through its in-built experiment engine.
* **Orchestration** - Routers supports deploying experiment workflows (through composable 'routers'). It is designed to work with pluggable pre- and post-processors and is backed by existing systems like CaraML Models for model endpoints. Routers takes care of all of the core Engineering aspects such as traffic routing, autoscaling, outcome logging, system monitoring and alerting.

CaraML Models project code name is Turing, which may show in code, SDK, API documentations

### **Experiments**

CaraML Experiments supports designing and managing experiment configurations in a safe and holistic manner. At run time, these configurations can be used (within the Router, or externally) to run the experiments and generate treatments. The experiments can be run either deterministically (A/B Experiments) or as a function of time (Switchback Experiments), or a combination of both (Randomized Switchbacks).

### **Pipelines**

CaraML Pipelines are a set of solutions to build data application systems like ETL processes and ML pipelines. CaraML Pipelines is powered by [Flyte](https://docs.flyte.org/en/latest/), an open-source workflow automation platform to create concurrent, scalable, and maintainable workflows for machine learning and data processing.

***

### Guides: Jump right in

Follow our handy guides to get started on the basics as quickly as possible:

{% content-ref url="projects/" %}
[projects](projects/)
{% endcontent-ref %}

{% content-ref url="module/model/user/generated/01_getting_started.md" %}
[model](module/model/user/generated/01_getting_started.md)
{% endcontent-ref %}

{% content-ref url="feature-store.md" %}
[feature-store.md](feature-store.md)
{% endcontent-ref %}

{% content-ref url="module/router/" %}
[router](module/router/)
{% endcontent-ref %}

{% content-ref url="module/experiment/how-to/01_getting_started.md" %}
[01\_getting\_started.md](module/experiment/how-to/01\_getting\_started.md)
{% endcontent-ref %}

{% content-ref url="pipelines.md" %}
[pipelines.md](pipelines.md)
{% endcontent-ref %}

## Deploying CaraML in your infrastructure?

Please refer to our deployment guide below to deploy CaraML into your own infrastructure.

{% content-ref url="deployment-guide/deploying-caraml.md" %}
[deploying-caraml.md](deployment-guide/deploying-caraml.md)
{% endcontent-ref %}
