page_title: Unified Pipeline Resources
page_description: List of supported resources
page_keywords: Deploy multi containers, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

# replicas
This resource type is used to control the number of instances of an application or service
that will be started in the target environment. This is used as an input resource to [manifest jobs](../jobs/manifest/) or [deploy jobs](../jobs/deploy/). 

You can create this resource by adding it to `shippable.resources.yml`
```
resources:
  - name: <string>                          	#required
    type: replicas                            	#required
    version:
      count: 1                                	#required
```

* `name` should be an easy to remember text string. This will appear in the visualization of this resource in the SPOG view and the list of resources in the Pipelines `Resources` tab. It is also used to refer to this resource in the jobs yml.

* `type` is always set to replicas

* `count` is an integer that represents the number of instances to run. 

A new version of this resource is created everytime anything in the version section changes.

##Overriding replicas
`replicas` can also be used to override settings that were set in an upstream stage of the pipeline.

For example, if you want to use different number of replicas in Test and Production environments, you can do so by overriding the resource.

<img src="../../images/resources/overrideReplicas.png" alt="Overriding replicas" style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

In the picture above, `deploy-test` takes `replicas-1` as an input. After testing, a release is created with the `release` job. This triggers production deployment with the `deploy-prod` job, which takes `replicas-2` as an input. For the production deployment, value of count in `replicas-2` will override the setting from `replicas-1`.



