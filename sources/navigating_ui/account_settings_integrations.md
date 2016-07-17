page_title: Navigating Shippable's Account Settings - Integrations UI
page_description: Overview of Shippable's Account Settings Integration section UI
page_keywords: User Interface, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

#Integrations
Shippable integrates with many third party services/platforms and you can leverage this by setting up an account integration and then using the integration at any point in your Shippable Continuous Integration or Delivery workflows.

This section lists all the integrations that have been added by this account. The information displayed for existing integrations are:

- Type: Icon representing the third party integration
- Name: Integration name defined at the time of creating the integration
- Master Name: Official name of the third party integration
- Master Type: Indicates the type of integration. The types are: scm (Source Control Management), Hub (Docker registries), Deploy (Container services, IaaS/PaaS providers), notification (Slack, email, Hipchat, etc.) and keys (SSH, PEM)
- Updated At: Displays the date the integration was updated
- `Edit` button: Edit the integration
- `Delete` button: Delete the integration

<img src="../images/account_settings_integrations.png" alt="List of integrations under Account Settings" style="width:700px;"/>

##Adding an account integration

- Click on the 'Account Settings' icon on the top right hand corner of Shippable portal.

- Click the 'Integrations' tab and then click 'Add Integration'

<img src="../images/add_account_integration.png" alt="Add Account Integration" style="width:700px;"/>

- Use the dropdown to select the desired integration and proceed with the configuration of that specific integration.

<img src="../images/add_integration_select.png" alt="Select Account Integration" style="width:700px;"/>

- Click on `Save`.

The integration should now show up in your `Integrations` tab. This integration will be available to all your projects and can be used for your workflows. Further details and examples of how a specific integration is used is available in the section for each integration.

---

## Deleting an account integration

To delete an integration: Simply click on the `Delete` button for the integration you want to delete.

Delete will fail if there are any projects actively using the integration, so make sure the integration is not being used in any project before deleting it.

<img src="../images/delete_account_integration.png" alt="Delete Account Integration" style="width:700px;"/>

---