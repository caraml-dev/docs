# Core Concepts

## Project

Project represents a namespace for a collection of CaraML resources, that belong to a specific team such as service accounts, Models, Routers, Pipelines etc. Project is one of the main building blocks for access control in CaraML. For creating a project, please refer to [Create a project](../user-guides/projects/create-a-project.md)

## Model

Model represent machine learning model. A model can have a type which determine how the model can be deployed. CaraML supports both standard model (XGBoost, SKLearn, Tensorflow, and PyTorch) and user-defined model (PyFunc, Custom Golang and Java model). Conceptually, model in CaraML is similar to a class in programming language. To instantiate a model you’ll have to create a model version.

### Model Version

Model version represents a snapshot of particular model iteration. A model version might contain artifacts which is deployable to CaraML. Each of a model version will have a version endpoint. CaraML Models supports up to 3 model version to be deployed at the same time. You'll also be able to attach information such as metrics and tag to a given Model Version.

### Model Version Endpoint

Model Version Endpoint is an URL associated with a Model Version deployment. Model Version Endpoint URL has following template:

```
http://<model_name>-<version>.<project_name>.<merlin_base_url>
```

Model Version Endpoint has several state:

* **pending**: The initial state of a Model Version Endpoint.
* **ready**: Once deployed, a Model Version Endpoint is in ready state and is accessible.
* **serving**: A Model Version Endpoint is in serving state if Model Endpoint has traffic rule which uses the particular Model Version Endpoint. A Model Version Endpoint could not be undeployed if its still in serving state.
* **terminated**: Once undeployed, a Model Version Endpoint is in terminated state.
* **failed**: If error occurred during deployment.

### Model Endpoint

Model Endpoint is a stable URL associated with a model. Model Endpoint URL has following template:

```
http://<model_name>.<project_name>.<merlin_base_url>
```

Model Endpoint can have a traffic rule which determine which Model Version Endpoint will receive traffic when request is received.

### Model Deployment and serving

Model deployment in CaraML is a process of creating a model service and its Model Version Endpoint. Internally, the deployment of the Model Version Endpoint is done via [Kaniko](https://github.com/GoogleContainerTools/kaniko) and [KFServing](https://github.com/kubeflow/kfserving).

There are two types of Model Version deployment, standard and python function (PyFunc) deployment. The difference is PyFunc deployment includes Docker image building step by Kaniko.

Model serving is the next step of model deployment. After we have a running Model Version Endpoint, we can start serving the HTTP traffic by routing the Model Endpoint to it.

![Model Serving](../.gitbook/assets/model\_deployment\_serving.svg)

## Transformer

Transformer is a service deployed in front of the model service which users can use to perform pre-processing and post-processing steps against the incoming requests before being sent to the model service. The benefits of using Transformer are users can abstract the transformation logic outside of their model and write it in a language more performant than python.

### Standard Transformer <a href="#standard-transformer" id="standard-transformer"></a>

Standard Transformer is a built-in pre and post-processing steps supported by CaraML. With standard transformer, it’s possible to enrich the model’s incoming request with features from feast and transform the payload so that it’s compatible with API interface provided by the model. Same transformation can also be applied against the model’s response payload in the post-processing step, which allow users to adapt the response payload to make it suitable for consumption.

### Custom Transformer <a href="#custom-transformer" id="custom-transformer"></a>

CaraML also supports Custom Transformer deployment. This transformer type enables the users to deploy their own pre-built Transformer service. The user should develop, build, and publish their own Transformer Docker image.

Similar to Standard Transformer, users can configure Custom Transformer from UI and SDK. The difference is instead of specifying the standard transformer configuration, users configure the Docker image and the command and arguments to run it.

## Feature Store

### **Entities:**

Entities are the objects in an organization on which features occur. They map to your business domain (users, products, transactions, locations).

### **Feature Tables:**

Defines a group of features that occur on a specific entity.

### **Features:**

Individual features within a feature table.

## Router

The router is the nucleus of the CaraML traffic routing system. It is responsible for coordinating the traffic routing to multiple model endpoints, invoking the pre- and post-processors, incorporating the response from the Experiment engine and logging of these responses.

### Request

Incoming message from the client to the CaraML traffic routing system.

### Response

The routing workflow involves the pre-processor (Enricher), the model endpoints, the Experiment engine and the post-processor (Ensembler), some of which are optional. Each component creates a response which becomes the request to the next component in the workflow (refer to How It Works below). In general, the Response refers to the final response from the CaraML routing system, after passing through all stages.

### Route

Model endpoint which may be a CaraML model or any arbitrary URL that can be reached from the CaraML infrastructure.

### Rule

Conditions determining which treatment to apply to a specific unit.

### Enricher

An optional service to perform arbitrary transformations on the incoming request or supplementing the request with data from external sources.

### Ensembler

An optional external service that accepts responses from the model endpoints altogether with the experiment configuration and responds back to the CaraML router with a final response. Exploration policies or combining responses from multiple models into one can be implemented here.

### Treatment

The set of configurations and actions to be applied to the current request which results in an outcome that can be evaluated.

### Unit

Smallest entity that can receive different treatments.

## Experiment

This section describes the main concepts related to CaraML Experiments component.

### Variables

Experiment Variables are input values that have an impact on the treatment generated by CaraML Experiments. These are retrieved from the incoming request and applied when running the experiment.

#### Segmenters

A segmenter is an attribute of the population considered for the experiment. CaraML Experiments supports the following segmenters:

* S2 IDs
* Days of the Week
* Hours of the Day

#### Segment

A combination of one or more segmenters with their specific values makes up a segment. Experiments are defined over segments and the experiment applicable to a given treatment request is determined by matching the segment. For example, consider the following experiments.

| Experiment Name | Segment                                 |
| --------------- | --------------------------------------- |
| exp\_1          | country=\[ID], service=\[ride]          |
| exp\_2          | country=\[ID], service=\[package, food] |

The parameters in the incoming treatment request must match each segmenter (AND) and one of the values in each values list (IN):

* A request containing `country=ID` and `service=ride` would match the first experiment. Similarly, A request containing `country=ID` and `service=package` (or `country=ID` and `service=food`) would match the second experiment.
* A request containing `country=ID` and `service=car` does not match any experiment. If the segment cannot be matched against the active experiments, an empty response is returned.

#### Randomization Unit

This a required value for A/B experiments and optional for Switchback experiments (may only be applicable to randomized switchbacks, depending on how the project is configured).

The value of the randomization unit in the request has an impact on the treatment generated. For example, this could be the pricing request id, which is used to randomly select a treatment from an A/B experiment's weighted list of treatment choices, where the weights are the traffic percentages assigned to the respective treatments.

### Experiment

An experiment is the set of configurations and filters that allow for systematically varying some independent variables to impact some other dependent variables. Experiment definitions comprise 3 types of information:

* Metadata such as the name, description, etc.
* Segment definition
* Treatment configurations

#### Experiment Orthogonality

Every request to CaraML Experiments to fetch a treatment for a given project and request parameters should be able to deterministically select no more than one experiment active in the given time. This is enforced by a property of the experiments called Orthogonality - for each pairwise combination of the experiments, there should be at least one segmenter in the experiments that has no overlapping values at the same "match strength". For more information on this and illustrations, please refer to the Experiment Hierarchy section below.

When active experiments are created and when inactive experiments are activated, Experiments runs these checks and their failure will result in the failure of the experiment creation/update.

#### Experiment Types

CaraML Experiments support various experiment types.

* **A/B Experiments** - Treatment assignment is randomized on the unit supplied in the request and one of the treatments in the experiment will be chosen at random, accounting for the traffic allocation for each treatment.
* **Switchback Experiments** - The main idea behind switchback experiments is that the experiment engine switches back and forth between the control and treatment configurations, per configured time interval. In CaraML Experiments, switchback experiments can have one or more treatments and the engine cycles through them, selecting one treatment for all requests in every time interval.
* **Randomized Switchback Experiments** - This is a hybrid between the A/B experiments and Switchbacks. These experiments are Switchbacks by nature (they have a time interval). In addition, they can have a traffic allocation on each of the treatments. Thus, at every new interval, the selection of the treatment is not cyclical, but randomized. In the default mode, all requests in a given time interval will still receive the same treatment. However, it is also possible to vary this.

#### Experiment Hierarchy

One of the greatest benefits of using CaraML Experiments to manage experiments is that, prior to generating the treatment from an experiment's configurations, the system handles the more complex task of 'selecting the right experiment' to run. Multiple simultaneous experiments can be scheduled on CaraML Experiments and the correct one is chosen at runtime, by matching the request parameters against the active experiments' configurations.

Where the incoming request may match multiple active experiments, the most granular experiment is chosen.

To understand the workings, let us consider an example project that uses the segmenters `country`, `geo_area` and `service` (in that order, as chosen in the Project Settings), and the following active experiments:

| Experiment Name | country | geo\_area  | service               |
| --------------- | ------- | ---------- | --------------------- |
| exp\_1          | ID      | 3 (Bali)   | 1 (Ride), 2 (Package) |
| exp\_2          | ID      | 3 (Bali)   | -                     |
| exp\_3          | ID      | -          | 1 (Ride)              |
| exp\_4          | ID      | 15 (Batam) | -                     |
| exp\_5          | ID      | -          | -                     |

Notes:

* `exp_1` is specific to Bali and the Ride/Package service types
* `exp_2` is the fallback experiment for Bali
* `exp_3` is the fallback experiment for the Ride service type
* `exp_4` is the fallback experiment for Batam
* `exp_5` is the fallback experiment for all Indonesia based requests

When the Fetch Treatment API is called, the user would have to supply the country, latitude & longitude (which will be used to match the geo\_area) and the service in the request parameters. The following experiments will be chosen based on the transformed parameters.

| Transformed Parameters | All Matched Experiments            | Chosen Experiment |
| ---------------------- | ---------------------------------- | ----------------- |
| (ID, Bali, Ride)       | `exp_1`, `exp_2`, `exp_3`, `exp_5` | `exp_1`           |
| (ID, Bali, Food)       | `exp_2`, `exp_5`                   | `exp_2`           |
| (ID, Bandung, Ride)    | `exp_3`, `exp_5`                   | `exp_3`           |
| (ID, Batam, Food)      | `exp_4`, `exp_5`                   | `exp_4`           |
| (ID, Bandung, Package) | `exp_5`                            | `exp_5`           |
| (SG, Singapore, Ride)  | -                                  | -                 |

**Optional Segmenters**

Segmenters registered in a project may be required or optional. Those that are optional can be supplied values in the experiment definition or can be left unset in the experiment, in which case, the experiment will apply to all values of that segmenter and we may also say that the segmenter is optional to the experiment.

**Inter-Segmenter Hierarchy**

In the first row, among `exp_2` and `exp_3`, there are 2 different optional segmenters. If `exp_1` did not exist, `exp_2` will be chosen because it has an exact match of the higher priority segmenter `service` (the **inter-segmenter hierarchy** is decided by the order in which the segmenters are chosen in the Project Settings).

**Revisiting Experiment Orthogonality**

The validation rules for configuring experiments is such that, no more than 1 experiment may be chosen at the time of treatment generation. This means that zero or more experiments may be matched by the transformed parameters but only (zero or) 1 of them can be ultimately filtered by the Fetch Treatment request. To achieve this, the system makes it impossible to schedule the following experiments, if the above experiments are also active for (parts of) the same duration:

| Experiment Name | country | geo\_area            | service  |
| --------------- | ------- | -------------------- | -------- |
| exp\_6          | ID      | 3 (Bali), 15 (Batam) | 1 (Ride) |
| exp\_7          | ID      | -                    | -        |

* `exp_6` cannot be created because there is already an exact experiment `exp_1` for Bali+Ride
* `exp_7` conflicts with `exp_5` - both geo\_area and service are optional in both experiments and the other segmenters (ID) overlap.

But we can create the experiment below, because there is no other experiment with an exact match for ID+Batam+Ride or ID+Batam+Package:

| Experiment Name | country | geo\_area  | service              |
| --------------- | ------- | ---------- | -------------------- |
| exp\_8          | ID      | 15 (Batam) | 1 (Ride), 2(Package) |

**Experiment Tiers**

We may often have a long-running experiment (say, for several weeks) for a certain segment and would like to run a short spike (say, for 1 day) to quickly test the impact of a different set of treatments for that segment. In such a scenario, we can make use of experiment tiers. CaraML Experiments only allow(s) one of 2 tiers:

* The **Default** tier which is the default value for all experiments
* The **Override** tier which will override the default experiment, if exists.

The Override experiments are not global overrides. They simply override the default experiment of a similar granularity. To illustrate this, let's consider the previous examples:

* If `exp_1` was in the default tier and `exp_2` in the override tier and a treatment request was made for (ID, Bali, Ride), the system would still select `exp_1` because it is more granular.
* If `exp_1` was in the default tier, `exp_6` can be created in the override tier (or vice versa). Of the 2, the experiment in the override tier will be chosen over the one in the default tier.

**A Note on S2IDs**

S2ID levels have an implicit hierarchy. The system accepts S2ID values at levels 10-14 and the more granular levels (14 is the most granular) will supersede the lower levels, by the same matching rules as above.

**Fetch Treatment API Hierarchy Resolution**

The following logic summarizes the experiment filtering mechanism adopted by the Fetch Treatment API:

1. Match all experiments for the given request. If 0 or 1 experiment matched, return.
2. Based on the inter-segmenter hierarchy, in that order, filter out weak matches if one or more exact matches exist for the segmenter.
3. If S2ID is used, select the experiment(s) with the most granular level among the matches.
4. At this point, we will either have one experiment or two (one in each tier). If we have 2 experiments, we pick the one in the override tier.

This way, the API will select exactly 1 experiment at the end of steps 2-4.

## Pipelines

### Tasks

You may refer to the definition of tasks in Flyte's documentation [here](https://docs.flyte.org/projects/cookbook/en/latest/auto/core/flyte\_basics/task.html#sphx-glr-auto-core-flyte-basics-task-py).

### Workflows

You may refer to the definition of workflows in Flyte's documentation [here](https://docs.flyte.org/projects/cookbook/en/latest/auto/core/flyte\_basics/basic\_workflow.html#sphx-glr-auto-core-flyte-basics-basic-workflow-py).

### Launch Plans

You may refer to the definition of launch plans in Flyte's documentation [here](https://docs.flyte.org/projects/cookbook/en/latest/auto/core/flyte\_basics/lp.html#sphx-glr-auto-core-flyte-basics-lp-py).
