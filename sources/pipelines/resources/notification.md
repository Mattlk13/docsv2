page_title: Unified Pipeline Resources
page_description: List of supported resources
page_keywords: Deploy multi containers, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

#notification

A `notification` resource is used to add a notification type so that you can send our notifications for the following events:

* Job starts (on_start)
* Job is completed successfully (on_success)
* Job failed (on_failure)

This resource type is only supported for jobs of type `runSh`. Email and Slack notifications are supported as of now.

You can create a notification resource by adding it to `shippable.resources.yml`

```
notification:
  - name: <string>								#required
    type: notification							#required
    integration: <string>						#required for Slack 
    pointer:									
      recipients:
        - "#beta"								#required
        - "@trriplejay"							#optional
```

The events for which this notification is sent out are configured in the jobs yml.


* `name` should be an easy to remember text string. This will appear in the visualization of this resource in the SPOG view and the list of resources in the Pipelines `Resources` tab. It is also used to refer to this resource in the jobs yml.

* `type` is always set to notification

* `integration` is only required for sending Slack notifications. The value should be set to the name of the integration that contains your credentials to connect to Slack. To learn how to add a Slack integration to your subscription, read the **Adding a Slack Integration** section on our [Slack integrations page](../../integrations/notifications/slack/) 

* `pointer` section provides information about recipients.

	* `method` is required for **email only** and should always be set to `email`. 

	* `recipients` is an array specifying who should receive notifications. For email notifications, include email addresses where you want to send notifications. For Slack notifications, include channel names or slack usernames where notifications should be sent. Slack channels/users should be entered in double quotes, with a leading # for channels and @ for users. For example, to send to a Slack room foo, specify `"#foo"` and to send to a person tom, specify `"@tom"`



