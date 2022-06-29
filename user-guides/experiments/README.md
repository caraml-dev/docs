# Experiments

CaraML Experiments support designing and managing experiment configurations in a safe and holistic manner. At run time, these configurations can be used (within the CaraML Router, or externally) to run the experiments and generate treatments. The experiments can be run either deterministically (A/B Experiments) or as a function of time (Switchback Experiments), or a combination of both (Randomized Switchbacks).

## Features

* **Reliable** - Inherent fault-detection rules help create experiments without conflicts
* **Customizable** - Every service has unique requirements. CaraML Experiments allows for defining flexible segmentation and experiment validation rules.
* **Fast** - 99p server-side latency (excluding the network latency between the calling service and CaraML Experiments) averages around 1 ms
* **Observable** - Resource utilization, treatment assignment and performance metrics are available on Prometheus

## Onboarding Process

For access to CaraML Experiments, users can self-onboard via the CaraML UI.

1\. Visit CaraML Project Landing Page, open the sidebar and click `Experiments`.&#x20;

![](../../.gitbook/assets/01\_mlp\_landing.png)

2\. You should see the following Landing page, if you have yet to setup the project for Experiments.&#x20;

![](../../.gitbook/assets/01\_experiments\_landing.png)

3\. Upon clicking 'here' in the previous page, you should see a form to input the necessary settings.&#x20;

![](../../.gitbook/assets/01\_settings\_create\_form.png)

4\. Enter a name for the Randomization Key and select the Segmenters.

* The order of the segmenters determines the priority of the segmenters when optional segmenters are used. For example, if the chosen segmenters are `s2_ids`, and `days_of_week` (in that order) and a given request matches 2 experiments - one where the `s2_ids` is optional and another one where the `days_of_week` is optional, the s2\_ids experiment (where there is an exact match of the s2\_ids) will be chosen. For more information and examples, please refer to the [Experiment Hierarchy](../../introduction/core-concepts.md#experiment-hierarchy) section in the Introduction page.
* Where the segmenter may be computed from several different (groups of) variables at runtime, also select the desired variable mapping. For example, `s2_ids` may be supplied as `s2_id` or computed from `latitude,longitude`. This must be specified in the settings.

5\. Click on Save. And voila! The onboarding is complete and you should see the configured settings. The project credentials (in particular, the `passkey`) would be required for running experiments ([CaraML Routers](../routers/) takes care of this if you are running the experiments through its routers).&#x20;

![](../../.gitbook/assets/01\_settings\_details.png)

