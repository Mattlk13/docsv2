page_title: Source control integrations
page_description: How to watch images using Shippable's Lighthouse Feature
page_keywords: lighthouse, shippable ci, documentation, shippable, watch docker images

# Source Control Management systems

Integrations for GitHub and Bitbucket do not need to be set up manually on Shippable. These are automatically configured for you when you sign in with your credentials for these services, or when you add them in the `Git identity` section of Account Settings.

---

## GitHub
Both public & private repositories are supported on Shippable. In order to integrate with your GitHub account, we automatically set up an Account integration when you log in using your GitHub credentials.

- For public repositories, you do not have to do anything to set this up further. You should be able to see all your public repos. Click the 'Sync' button if you don't see your public repo.
- For private repositories, you need to authorize Shippable to access them. You can do this by following the outlined steps:

  - Click on the Account settings (gear icon on the top right hand navigation bar)
  - In the 'Accounts' tab and under 'Git Identities' section, click 'Enable' under 'GitHub' 'Click to enable private access'.

<img src="../images/int_scm_mv_enable_pvtrepo_access.gif" alt="Enable access to Private Repositories" style="width:700px;"/>

---  

## Bitbucket
In order to integrate with your Bitbucket account, we automatically set up an Account integration when you log in using your Bitbucket credentials. You do not have to do anything to set this up further.

Shippable also integrates with Bitbucket to build pull requests. Read [our documentation](ci_configure/#bitbucket) for details on setting it up.

---

## GitHub Enterprise
We do not support signing in to Shippable directly with your GitHub Enterprise credentials. In order to use Shippable for your GitHub Enterprise repositories, you will need to sign in with GitHub or Bitbucket and then add an account integration as follows :

- Click on the gear icon for Account Settings in the top navigation bar. Select the `Integrations` tab.
- Click on `Add Integration` and from the dropdown for **Integration type**, select `GitHub Enterprise`
- Enter a name for this integration in the **Integration name** textbox.
- Enter the URL for your GitHub Enterprise instance. The URL should be in the format https://(git hub enterprise URL)/api/v3
- You will need to add a token from your GitHub Enterprise account with the right permissions. To do this -
    - Go to your GitHub Enterprise account settings and in the left menu, select
   `Personal access tokens`.
    - Click on `Generate token` and on the Generate Token page, select the following permissions as shown below:

    <img src="../images/int_github_enterprise_permissions.png" alt="GitHub Enterprise Permissions" style="width:700px;"/>

    - Click on `Generate token`, and copy the generated token immediately. This is important since you will not see the token once you navigate away from this page.
    - Paste the token in your Integration on Shippable and click on `Save`.   
- After adding a GitHub Enterprise integration, you should go to the Account Settings tab and `Sync` your account.
- Go to your Shippable dashboard and check to make sure you have your GitHub Enterprise subscriptions in the CI dropdown.

You can now enable projects, run CI, etc just like with GitHub or Bitbucket subscriptions.

---

## GitLab
In order to integrate with GitLab, you need to sign up to Shippable using your GitHub/Bitbucket account and then add GitLab as an account integration as follows:

- Click on the gear icon for Account Settings in the top navigation bar. Select the `Integrations` tab.
- Click on `Add Integration` and from the dropdown for **Integration type**, select `GitLab`
- Enter a name for this integration in the **Integration name** textbox.
- Enter the URL for your GitLab instance. The URL should be in the format https://(GitLab URL)/api/v3. For example, if you're using gitlab.com, this will `https://gitlab.com/api/v3`
- You will need to add a private token from your GitLab account. To do this -
    - Go to your GitLab profile settings and in the left menu, select
    `Account`
    - Copy the private token provided
    - Paste the token in your integration on Shippable and click on `Save`.
- After adding the GitLab integration, you should go to the Account Settings tab and `Sync` your Account.
- Go to your Shippable dashboard and check to make sure your GitLab subscriptions are available in the CI dropdown.

---

## Bitbucket Server
In order to integrate with Bitbucket Server, Shippable provides an addon which needs to be installed by the Bitbucket Server administrator. This is available in the [Atlassian Marketplace](https://marketplace.atlassian.com/plugins/shippable/cloud/overview). Once installed, sign in to Shippable using your GitHub/Bitbucket account and then add Bitbucket Server as an account integration as follows:

- Click on the gear icon for Account Settings in the top navigation bar. Select the `Integrations` tab.
- Click on `Add Integration` and from the dropdown for **Integration type**, select `Bitbucket Server`.
- Enter a name for this integration in the **Integration name** textbox.
- Enter the username used to login to your Bitbucket Server account in the **Username** textbox.
- Enter the URL for your Bitbucket Server instance in the 'URL' textbox. **Ensure there are no trailing slashes here**. Read the [FAQ](ci_faq/#i-cannot-start-a-manual-build-for-my-bitbucket-project-why-is-it-not-working) on this topic for more details. 
- You will need to add a private token from your Bitbucket Server account. To do this -
    - Go to your Bitbucket Server account settings and in the left menu, select
    `Shippable Token`
    - Copy the private token provided
    - Paste the token in the **Token** textbox on Shippable and click on `Save`.
- After adding the Bitbucket Server integration, you should go to the Account Settings tab and `Sync` your Account.
- Go to your Shippable dashboard and check to make sure your Bitbucket Server subscriptions are available in the CI dropdown.

For more details, read the stepwise tutorial - [Using Shippable with Bitbucket Server](ht_using_bitbucket_server.md)


---
