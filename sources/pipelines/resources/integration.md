page_title: Unified Pipeline Resources
page_description: List of supported resources
page_keywords: Deploy multi containers, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

#integration

Shippable is designed to separate out sensitive authentication information from resources. 
This is done to ensure there are no encryption/decryption or permissions issues when you move things around i.e. moving resource definitions from one repository to another, or if the person who created the pipeline is no longer the member of the team etc. Integrations are specified as a property in the YML definition for resources that connect to third party services. 

An `integration` resource contains your credentials to connect to any [supported third party platform or provider](../../integrations/overview/). This resource is used an input `IN` to [runSh jobs](../jobs/runSh/).

You can create an integration resource by adding it to `shippable.resources.yml`

```
- name: <string>
  type: integration
  integration: <string>
```

* `name` should be an easy to remember text string. This will appear in the visualization of this resource in the SPOG view and the list of resources in the Pipelines `Resources` tab. It is also used to refer to this resource in the jobs yml.

* `type` is always set to integration

* `integration` should be the name of the integration that connects to the third party platform or service you want to connect to. For a complete list of supported third party integrations, visit our [Integrations overview page](../../integrations/overview/), click on a specific integration, and read the **Adding the Account Integration** section.

