page_title: Unified Pipeline Resources
page_description: List of supported resources
page_keywords: Deploy multi containers, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

# image

An `image` resource is used to add a reference to a docker image to your pipeline. It is used as an input for [manifest jobs](../jobs/manifest/).

You can create an `image` resource by adding it to `shippable.resources.yml`:

```
resources:
  - name: <string>                           	 #required
    type: image                               	 #required
    integration: <string>                   	 #required
    pointer:
      sourceName: "org/repo"                    #required
    seed:
      versionName: "<string>"                	 #required
```

* `name` should be an easy to remember text string. This will appear in the visualization of this resource in the SPOG view and the list of resources in the Pipelines `Resources` tab. It is also used to refer to this resource in the jobs yml. If you have spaces in your name, you'll need to surround the value with quotes, however, as a best practice we recommend not including spaces in your names.

* `type` is always set to 'image'.

* `integration` should be the name of the integration that connects to the Docker Registry provider where the image is located. To learn how to create integrations for a specific Docker Registry, please select from the list below and read the **Adding an integration** section on that page:

	- [Docker Hub](../../integrations/imageRegistries/dockerHub/)
	- [Docker Private Registry](../../integrations/imageRegistries/privateRegistry/)
	- [Docker Trusted Registry](../../integrations/imageRegistries/dockerTrustedRegistry/)
	- [Google Container Registry (GCR)](../../integrations/imageRegistries/gcr/)
	- [Amazon Elastic Container Registry (ECR)](../../integrations/imageRegistries/ecr/)
	- [Quay.io](../../integrations/imageRegistries/quay/)


* `pointer` section provides information about the image.
	* `sourceName` is the fully qualified name of the image. This is dependent on the registry where the image is located. For Docker Hub, this can be <repo name>/<image name>, e.g. manishas/demoImage

* `versionName` is usually set to image tag. The seed versionName sets initial tag for the image.
