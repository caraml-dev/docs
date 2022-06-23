# Connecting to Models

## Python SDK

* Log Model into CaraML Models
* Manage Model Version Endpoint and Model Version Endpoint
* Manage Batch Prediction Job

{% content-ref url="python-sdk.md" %}
[python-sdk.md](python-sdk.md)
{% endcontent-ref %}

## Models CLI

* Deploy the Model Endpoint
* Undeploy the Model Endpoint
* Scaffold a new PyFunc project

{% content-ref url="models-cli.md" %}
[models-cli.md](models-cli.md)
{% endcontent-ref %}

## Client Libraries

CaraML Model provides [Go client library](https://github.com/eric-lidong/merlin/blob/main/api/client/client.go) to deploy and serve ML model in production.

To connect to Models deployment, the client needs to be authenticated by Google OAuth2. You can use `google.DefaultClient()` to get the Application Default Credential.

```go
googleClient, _ := google.DefaultClient(context.Background(), "https://www.googleapis.com/auth/userinfo.email")

cfg := client.NewConfiguration()
cfg.BasePath = "http://merlin.dev/api/merlin/v1"
cfg.HTTPClient = googleClient

```
