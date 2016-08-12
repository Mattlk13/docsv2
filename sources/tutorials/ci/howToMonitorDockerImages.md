page_title: Monitor your Docker images for updates
page_description: Overview of Shippable's feature that allows you to monitor your Docker images for updates
page_keywords: User Interface, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc, lighthouse

#Monitor your Docker images

##Overview

Have you ever wasted your time debugging a broken build only to realize a few frustrated hours later that it was due to something that changed in one of the images you depend on?

We have a feature to your rescue!

Shippable lets you monitor any Docker image from any registry and be notified if the image changes. This is very powerful since you can now set up Shippable to automatically build all dependent projects when an upstream image is updated.

This feature is available to all Shippable users for free. You can monitor any number of images at no cost.

We're actively expanding the functionality to include monitor dependencies that go beyond just Docker images, so stay tuned!

You can check out this feature by signing in to [Shippable](http://www.shippable.com) and clicking on the 'Account Settings' (gear icon on the top right hand navigation bar) and clicking the 'Images' tab.

---

##Adding an image

To monitor an image and receive notification when the image is updated in the registry, follow the steps below:

1. Create an account integration for the Docker registry where the image is to be monitored. (Instructions here)
2. Create an account integration for the type of notification you want to receive when the image changes. You can set up [Slack](/continuous_integration/notifications/slack/#Configuring-slack-notifications-to-monitor-Docker-Images/), [HipChat](/continuous_integration/notifications/hipchat/#Configuring-hipchat-notifications-to-monitor-Docker-Images/), [IRC](/continuous_integration/notifications/irc/#Configuring-IRC-notifications-to-monitor-Docker-Images/), or [Email](/continuous_integration/notifications/email/#Configuring-email-notifications-to-monitor-Docker-Images/) notification.
3. Go to your 'Account Settings' (gear icon on the top right hand navigation bar) and click the 'Images' tab.
4. Click on Add image and complete the following:
     - Image Name: your_repo_name/image_name (the image from either DockerHub or GCR)
     - Hub Integration: The Docker registry integration that has permissions to access this image. You can create a new one if you don't have one already configured in your Account Settings.
     - Click on the Save Image button
     - Next, enter the type of notification you want to receive each time the image changes. This can be email, Slack, HipChat or IRC. Enter the account integration details.
     - Click on Ok
     - You will start seeing the status of the image. A green indicator shows that we could successfully sync the image and will be able to send notifications when the image changes.

Images are 'watched' every 20 mins, so your notification will be sent 20 mins after the image is updated, in the worst case.

---

##Tags

Once configured, Shippable periodically polls the registry and the tags are updated when there is any update to the image in the source registry. You will see the latest tag against the image when the image is synced.

---

##Updating an image

To update an image or change the notification settings:

1. Go to your 'Account Settings' (gear icon on the top right hand navigation bar) and click the 'Images' tab.
2. You will see the list of images being watched.
3. Click on the image_name to update the notification setting

---

##Deleting an image

To delete an image from being monitored:

1. Go to your 'Account Settings' (gear icon on the top right hand navigation bar) and click the 'Images' tab.
2. You will see the list of images being watched.
3. Click on the Delete button for the image(s) you want to stop monitoring

---
