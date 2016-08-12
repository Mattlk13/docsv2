page_title: Navigating Shippable's Account Settings - API UI
page_description: Overview of Shippable's Account Settings API section UI
page_keywords: User Interface, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

#API Tokens
This is where you generate, view and manage access tokens to use our [API](/api/api/).

##Adding a token
To add a token:

- Click the 'Add Tokens' button.
- Enter a token name and click on `Add` to create a new token.
- IMPORTANT: Remember to copy the token. For security reasons, the token will never
be displayed again.

<img src="../../images/accountSettings/tokenAdd.png" alt="Add API token on Shippable"
style="width:700px;"/>

---

##Deleting a token
To delete a token, click on the `Delete` button next to the token you want to delete.

<img src="../../images/accountSettings/tokenDelete.png" alt="API token on Shippable"
style="width:700px;"/>

---

**Please remember to keep your token safe and do not share it with anyone.** If your token get compromised, API calls can be made on your behalf and compromise the security of your Shippable account.
Treat your API token like an admin password.


In case this happens, just deleting the token will remove all access to the token.

You can create a new one to replace it and make sure you update it in all external services that might have been using it.
---
