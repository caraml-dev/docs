# Creating a Router

## Create a router

A Router in the CaraML ecosystem represents an ML experiment and holds the configuration for the traffic routing, pre/post-processors, incorporating the response from the Experiment engine and logging. It is built on our Fiber traffic routing library written in Golang, and sources its configuration when it starts up.

#### Setting Up An Experiment

In CaraML, your will need to create a router with an optional Experiment Engine, Pre-processor (Enricher) and Post-processor (Ensembler) configured based on your requirements. After configuring and deploying the router, you will receive a URL to which you are able to send your experimentation requests.

#### Navigate to the Create Router UI

* Open the CaraML project homepage.
* Choose a project in which you want to create your router. If such a project does not exist, you can [create a project](../../projects/create-a-project.md)

![Project selection](../../../.gitbook/assets/projects\_dropdown.png)

* Choose “Create Router”.

![Create Routers](../../../.gitbook/assets/create\_router\_button.png)

#### Configure router

Now that we have navigated to the create a router page, we can continue to configure the router.

{% content-ref url="configure-general-settings.md" %}
[configure-general-settings.md](configure-general-settings.md)
{% endcontent-ref %}

{% content-ref url="configure-routes.md" %}
[configure-routes.md](configure-routes.md)
{% endcontent-ref %}

{% content-ref url="configure-traffic-rules.md" %}
[configure-traffic-rules.md](configure-traffic-rules.md)
{% endcontent-ref %}

{% content-ref url="configure-autoscaling.md" %}
[configure-autoscaling.md](configure-autoscaling.md)
{% endcontent-ref %}

{% content-ref url="configure-experiment-engine.md" %}
[configure-experiment-engine.md](configure-experiment-engine.md)
{% endcontent-ref %}

{% content-ref url="configure-enricher.md" %}
[configure-enricher.md](configure-enricher.md)
{% endcontent-ref %}

{% content-ref url="configure-ensembler.md" %}
[configure-ensembler.md](configure-ensembler.md)
{% endcontent-ref %}
