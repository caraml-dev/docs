# Adding transformer config

In CaraML ecosystem, Transformer is a service deployed in front of the model service which users can use to perform pre-processing and post-processing steps into the incoming requests before being sent to the model service. The benefits of using Transformer are users can abstract the transformation logic outside of their model and write it in a language more performant than python.

Currently, CaraML Models has two types of Transformer: Standard and Custom Transformer.

{% content-ref url="standard-transformer/" %}
[standard-transformer](standard-transformer/)
{% endcontent-ref %}

{% content-ref url="custom-transformer.md" %}
[custom-transformer.md](custom-transformer.md)
{% endcontent-ref %}
