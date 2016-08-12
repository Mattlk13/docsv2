page_title: Integrating Email on the Shippable platform
page_description: Configure Email to send out email notifications for Continuous Integration/Continuous Delivery actions
page_keywords: Integration, Integrate, Slack, Notify, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, email, HipChat, IRC

#Email
By default, we send email notifications to the last committer and project owner when a build fails, or the status changes from failed to passed.

We get the email address from your source control management system (GitHub/Bitbucket).

To customize the email notifications, you'll need to configure options in the `shippable.yml` file. There is no setting up of email integration through the UI.

##Configure email notification options in the `shippable.yml`
To configure email notifications for your project, add the following to the `shippable.yml` file for that project.
```
integrations:
  notifications:
    - integrationName: email
      type: email
      recipients:
        - exampleone@org.com
        - exampletwo@org.com
      branches:
        only:
          - master
          - dev
      on_success: always
      on_failure: always
```

While the above is a sample code for your `shippable.yml`, use the descriptions of each field below to modify the `yml` and tailor it to your requirements.

- `integrationName` value is always `email` since it is not configured in the UI - 'Account Settings' and 'Subscription' settings.
- `type` is `email`.
- `recipients` specifies the email addresses you want to send build status notifications to. This overrides the default setting of 'last committer' and 'project owner(s)' email address that we get from your source control management system (GitHub/Bitbucket). NOTE: We do not use the email address specified in your 'Account Settings' for notifications.
     - To specify 'last committer' and 'project owner(s)' as part of this list, you can use --last_committer and --owners.
     - For single recipient, use the format `recipients: example@org.com`
- [optional] `branches` allows you to choose the branches you want to send notifications for. By default, notifications are sent for all branches. The `only` tag should be used when you want to send notifications to specific branches. You can also use the `except` tag to exclude specific branches.
- [optional] You can set the following options for the `on_success`, `on_failure` tags:
     - The value `change` for `on_success` or `on_failure` fields means you will receive notifications only when the build status changes to success or failure respectively.
     - `always` means that you will always receive a notification for that build status.
     - `never` means that you will never receive a notification for that build status.
     - By default, `on_success` is set to `change` and `on_failure` is set to `always` when these tags have not been specified in the `shippable.yml` file.
- [optional] You can set the following options for the `on_start`, `on_pull_request` tags:
     - Setting the value to `always` means that you will always receive a notification for build start/pull request.
     - Setting the value to `never` means that you will never receive a notification for that build start/pull request.
     - By default, `on_start` is set to `never` and `on_pull_request` is set to `always` when these tags have not been specified in the `shippable.yml` file.

Check our blog ["Notifying CI failure/success status on Email and Slack"](http://blog.shippable.com/notifying-ci-failure/success-status-on-email-slack) for multiple scenarios.

---

##Configuring email notifications to monitor Docker Images

You can add an Email account integration to receive notifications for monitored Docker images. To do this:

1. Ensure you have configured [the monitoring of a Docker image](/navigatingUI/accountSettings/images/).
2. Go to 'Account Settings' and click the 'Images' section.
3. Select the 'Image' you want to configure the email notifications.
4. Under the 'Notifications' section, use the dropdown for the 'Channel' field and select `email`.
5. Enter the email address in the 'recipients' field. For example: janedoe@org.com
6. Click the `Add Item` button.
7. [Optional] Add additional recipients.
8. Click the `Save` button.

<img src="/ci/images/integrations/notifications/email/monitorImages.png" alt="Configure an email address to be notified while monitoring a Docker image" style="width:700px;"/>

---

##Turn off email notification
If you do not want to get notified for any reason, you can turn off email notifications with the following in your `shippable.yml`:

```
notifications:
  - integrationName: email
    type: email
    on_success: never
    on_failure: never
    on_pull_request: never
```
---
