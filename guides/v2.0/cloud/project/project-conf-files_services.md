---
layout: default
group: cloud
subgroup: 090_configure
title: services.yaml
menu_title: services.yaml
menu_order: 55
menu_node:
level3_menu_node: level3child
level3_subgroup: services
version: 2.0
github_link: cloud/project/project-conf-files_services.md
---

We provide a `services.yaml` file to configure all of your services supported and used by {{site.data.var.ece}}. These services include MySQL, PHP, Redis, Solr (for 2.0.X), and so on. You don't need to subscribe to external service providers.

This file is located at `.magento/services.yaml` in your project.

<div class="bs-callout bs-callout-info" id="info">
  <p>When you push your Git branch, our deploy script uses the values defined by configuration files in the <code>.magento</code> directory. After deployment, the script deletes the directory and its contents. Your local development environment isn't affected.</p>
</div>

To see an example, see this [sample `services.yaml` file](https://github.com/magento/magento-cloud/blob/master/.magento/services.yaml){:target="_blank"}.

**For Pro:** Changes you make using `.yaml` files affect your [Integration environment]({{page.baseurl}}cloud/reference/discover-arch.html#cloud-arch-int) only. For technical reasons, your [Staging]({{page.baseurl}}cloud/reference/discover-arch.html#cloud-arch-stage) and [Production]({{page.baseurl}}cloud/reference/discover-arch.html#cloud-arch-prod) environments do not use `.yaml` files. To make these changes in these environments, you must create a [Support ticket]({{page.baseurl}}cloud/bk-cloud.html#gethelp).


The following sections discuss properties in `services.yaml`.

## How this file works {#howitworks}
The `.magento.app.yaml` and `services.yaml` files set the services, applications, and configurations to build and include in an environment. If you add services with specific versions, the initial push and deployment of your branches with these updated files directs the PaaS environment to provision the environment with those services. When you make changes to the services, the environment updates.

This affects the following environments:

* All Starter environments including Production `master`
* Pro Integration environments

To install and update services in Pro Staging and Production environments (IaaS), you must enter a [Support ticket]({{page.baseurl}}cloud/bk-cloud.html#gethelp). Indicate the service changes needed and your updated `.magento.app.yaml` and `services.yaml` files in the ticket.

## Default services {#cloud-yaml-services-default}
Your Git branch includes the following default `services.yaml` file:

	mysql:
	   type: mysql:10.0
	   disk: 2048

	redis:
	   type: redis:3.0

	solr:
	   type: solr:4.10
	   disk: 1024

Modify this file to use specific and additional services in your deployment. See the [`type`](#cloud-yaml-services-type) section to see the services we support and deploy for you if you add them to the file.

## Service values {#services}
To add a service, you add the following data to services.yaml:

  name:
     type: name:version
     disk: value

For example:

  mysql:
     type: mysql:10.0
     disk: 2048

### `name` {#cloud-yaml-services-name}
`name` identifies the service in the project. The `name` can consist only of lower case alphanumeric characters: `a`&ndash;`z` and `0`&ndash;`9`. For example, Redis is entered as redis.

You can have multiple instances of each service type. For example, you could have multiple Redis instances. For example, we use multiple Redis instances, one for session and one for cache.

  redis:
     type: redis:3.0

  redis2:
     type: redis:3.0

Be aware, if you rename a service in `services.yaml`, the following is **permanently removed**:

* The existing service before creating a new service with the new name you specify.
* All existing data for the service is removed. We strongly recommend you [snapshot your environment]({{page.baseurl}}cloud/project/project-webint-snap.html) before you change the name of an existing service.

### `type` {#cloud-yaml-services-type}
The `type` of your service in the format `type:version`

We support and deploy the following services for you:

*	[`mysql`]({{page.baseurl}}cloud/project/project-conf-files_services-mysql.html) version `10.0`
*	[`redis`]({{page.baseurl}}cloud/project/project-conf-files_services-redis.html) versions `2.8` and `3.0`
*	[`solr`](http://devdocs.magento.com/guides/v2.0/cloud/project/project-conf-files_services-solr.html) version `4.1`
*	[`rabbitmq`]({{page.baseurl}}cloud/project/project-conf-files_services-rabbit.html) version `3.5`

### `disk` {#cloud-yaml-services-disk}

`disk` specifies the size of the persistent disk storage in MB allocated to the service.

For example, the current default storage amount per project is 5GB, or 5120MB. You can distribute this amount between your application and each of its services. See [`.magento.app.yaml`]({{page.baseurl}}cloud/project/project-conf-files_magento-app.html#cloud-yaml-platform-rel).

## Using the services
For services to be available to an application in your project, you must specify [*relationships*]({{page.baseurl}}cloud/project/project-conf-files_magento-app.html#cloud-yaml-platform-rel) between applications and services in `.magento.app.yaml`.

#### Related topics
*	[Get started with a project]({{page.baseurl}}cloud/project/project-start.html)
*	[`.magento.app.yaml`]({{page.baseurl}}cloud/project/project-conf-files_magento-app.html)
*	[Set up MySQL service]({{page.baseurl}}cloud/project/project-conf-files_services-mysql.html)
*	[Set up Redis service]({{page.baseurl}}cloud/project/project-conf-files_services-redis.html)
*	[Set up Solr service](http://devdocs.magento.com/guides/v2.0/cloud/project/project-conf-files_services-solr.html)
*	[Set up RabbitMQ service]({{page.baseurl}}cloud/project/project-conf-files_services-rabbit.html)
*	[`routes.yaml`]({{page.baseurl}}cloud/project/project-conf-files_routes.html)
