---
layout: default
group: cloud
subgroup: 07_project
title: Basic project information
menu_title: Basic project information
menu_order: 4
menu_node: 
level3_menu_node: level3child
level3_subgroup: project
github_link: cloud/project/project-webint-basic.md
---

## Basic project information {#project-basic}
This topic discusses how to use the Web Interface to:

*	Log in to the project
*	Access your site
*	Get started configuring the project
*	Get started configuring an environment
*	View project history

## Log in to your project {#project-login}
The Web Interface URL for your project is available in the following ways:

*	Your welcome e-mail
*	The Magento Enterprise Cloud Edition command-line interface (CLI)

### Find the project URL using the CLI
To find the project URL using the CLI:

1.	Log in to the machine on which your SSH keys are stored.
2.	Log in to your project:

		magento-cloud login
3.	Enter the following command:

		magento-cloud project:list

	A sample result follows:

		Your projects are:

		+---------------+--------------+---------------------------------------------------+
		| ID            | Title        | URL                                               |
		+---------------+--------------+---------------------------------------------------+
		| pwga254dhx97o | Magento 2    | https://us.magento.cloud/#/projects/pwga254dhx97o |
		+---------------+--------------+---------------------------------------------------+
4.	Enter the value in the URL column in a web browser.

### Log in to your project
In a web browser, enter the project URL from your welcome e-mail or that you found using the CLI. When prompted, log in to your project using Bitbucket, GitHub, Google, or a e-mail address and password.

![Log in to a project]({{ site.baseurl }}common/images/cloud_project-login.png){:width="450px"}

## Access your project and environments {#project-access}
The Web Interface provides several ways to access your project and environments:

*	Your storefront URL (every environment, or branch, has a different URL)
*	Secure Shell (SSH), a way to interact with services using a command terminal
*	Clone the project using the Magento Enterprise Cloud Edition CLI
*	Clone the project using Git

To access projects and environments:

1.	[Log in to your project](#project-login).
2.	Hover the mouse pointer on **Access Project** as the following figure shows:

	![Access your project by URL or SSH]({{ site.baseurl }}common/images/cloud_project-access.png)
3.	For example, to view your storefront, click the **Web Access** link.

	For more information about using SSH, see [SSH to an environment]({{ site.gdeurl }}cloud/env/environments-start.html#env-start-ssh).

## Get started configuring your project {#project-conf}
Configuring a project means:

*	Managing users
*	Using a deploy key to pull code from a private repository

To configure your project:

1.	[Log in to your project](#project-login).
2.	Click ![configure your project]({{ site.baseurl }}common/images/cloud_edit-project.png) (Configure project) next to the project name.
3.	See one of the following for more information:

	*	[Manage users]({{ site.gdeurl }}cloud/admin/admin-user-admin.html)
	*	[Pull code from a private Git repository]({{ site.gdeurl }}cloud/project/project-priv-repos.html)

## Get started configuring an environment {#project-conf-env}
Configuring an enviornment means:

*	Environment settings
*	Configuring environment variables
*	Configuring routes
*	Managing users

To configure your project:

1.	[Log in to your project](#project-login).
2.	Click **Configure environment** under the project name, as the following figure shows.

	![Configure environment]({{ site.baseurl }}common/images/cloud_project-conf-env.png)
3.	See one of the following for more information:

	*	[Environment settings](#project-conf-env-set)
	*	[Set environment variables](#project-conf-env-var)
	*	[Configure routes](#project-conf-env-route)
	*	[Manage users]({{ site.gdeurl }}cloud/admin/admin-user-admin.html)

### Environment settings {#project-conf-env-set}
The following table shows available environment settings.

<table>
	<tbody>
		<tr>
			<th>Option</th>
			<th>Description</th>
		</tr>
	<tr>
		<td>Environment status</td>
		<td>An environment can be either <em>active</em> or <em>inactive</em>. You'll do most of your work in an active environment. After merging an environment with its parent, you can optionally delete the environment, making it inactive. To delete an environment, click <strong>Delete</strong>. You can active an inactive environment later.</td>
	</tr>
	<tr>
		<td>Outgoing emails</td>
		<td>Setting to <strong>On</strong> means that code in your environment can send and receive e-mails (for example, using PHP <code>email()</code> function. </td>
	</tr>
	<tr><td>HTTP access control</td>
	<td>Setting to <strong>On</strong> enables you to configure security for the project's Web Interface using a login and also IP address access control.</td>
	</tr>
</tbody>
</table>

### Set environment variables {#project-conf-env-var}
As discussed in [Overview of environment variables]({{ site.gdeurl }}cloud/env/environment-vars_over.html), environment variables are settings specific to an environment. Variables can be either text or JSON format.

For example, you can change the Magento Admin administrative password using environment variables as follows:

1.	Click **Add Variable**.
2.	In the **Name** field, enter `ADMIN_PASSWORD`.
3.	In the **Value** field, enter the administrator's password.

	The following figure shows an example.

	![Set environment variables]({{ site.baseurl }}common/images/cloud_env-var.png)
4.	Click **Add Varible**.
5.	Wait while the environment deploys.

### Configure routes {#project-conf-env-route}
As discussed in [routes.yaml]({{ site.gdeurl }}cloud/project/project-conf-files_routes.html), routes (or URLs) used to access your Magento storefront. See that section for details about what the options mean.

The following figure shows an example.

![Configure a route]({{ site.baseurl }}common/images/cloud_routes.png)