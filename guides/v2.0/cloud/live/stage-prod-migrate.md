---
layout: default
group: cloud
subgroup: 160_deploy
title: Migrate and deploy code, static files, and data
menu_title: Migrate and deploy code, static files, and data
menu_order: 50
menu_node:
version: 2.0
github_link: cloud/live/stage-prod-migrate.md
---

#### Previous step:
[Prepare to deploy to Staging and Production]({{ page.baseurl }}cloud/live/stage-prod-migrate-prereq.html)

To migrate your database and static files to Staging and Production:

* [Deploy code](#code)
*	[Migrate static files](#cloud-live-migrate-static)
*	[Migrate the database](#cloud-live-migrate-db)

If you encounter errors or need to make changes, complete those updates on your local. Push the code changes to the Integration environment. Deploy the updated `master` branch again. See instructions in the [previous step]({{ page.baseurl }}cloud/live/stage-prod-migrate.html).

## Deploy to Staging and Production {#code}
The Project Web Interface provides full features to create, manage, and deploy code branches in your Integration, Staging, and Production environments for Starter and Pro plans. You can also use SSH and CLI commands to complete these process. Previously for Pro plans, you could only use SSH and CLI commands for Staging and Production.

For Pro projects created **after 10-23-2017**, deploy the Integration `master` branch you created to Staging and Production.

1. [Log in](https://accounts.magento.cloud) to your project.
2. Select the Integration branch.
3. Select the **Merge** option to deploy to Staging. Complete all testing.
4. Select the Staging branch.
5. Select the **Merge** option to deploy to Production.

{% include cloud/wings-management.md %}

For Starter, deploy your development branch you created to Staging and Production.

1. [Log in](https://accounts.magento.cloud) to your project.
2. Select the prepared code branch.
3. Select the **Merge** option to deploy to Staging. Complete all testing.
4. Select the Staging branch.
5. Select the **Merge** option to deploy to Production.

![Use the merge option to deploy]({{ site.baseurl }}common/images/cloud_project-merge.png)

## Deploy using SSH {#ssh}
If you prefer to use CLI for deploying, you will need to configure additional SSH settings and Git remotes to use commands. You can SSH into the Staging and Production environments to push the `master` branch.

You'll need the SSH and Git access information for your project.

For Starter projects, locate the SSH and Git information through the Project Web Interface.

For Pro projects created **after 10-23-2017**, locate the SSH and Git information through the Project Web Interface.

For Pro projects created **before 10-23-2017**, the formats are as follows:

*	Git URL format:

	*	Staging: `git@git.ent.magento.cloud:<project ID>_stg.git`
	*	Production: `git@git.ent.magento.cloud:<project ID>.git`

*	SSH URL format:

	*	Staging: `<project ID>_stg@<project ID>.ent.magento.cloud`
	*	Production: `<project ID>@<project ID>.ent.magento.cloud`

After that is set up, you can SSH into the environment and use Git commands to push the branches.

### SSH and pull the Git branch {#git}
This information is for Pro projects created **before 10-23-2017**.

1. Open an SSH connection to your Staging or Production environment:

    * Staging: `ssh -A <project ID>_stg@<project ID>.ent.magento.cloud`
    * Production: `ssh -A <project ID>@<project ID>.ent.magento.cloud`
2. Pull the `master` branch to the server.

        git pull origin master

### SSH and merge the Git branch
This information is for Pro projects created **after 10-23-2017**. The Integration branch is the `master` branch for your code base. To deploy to Staging and Production, you can merge or sync your `master` code to the `staging` and `production` branches. 

## Deploy migrate static files {#cloud-live-migrate-static}
You will migrate {% glossarytooltip 363662cb-73f1-4347-a15e-2d2adabeb0c2 %}static files{% endglossarytooltip %} from your `pub/media` directory to Staging or Production.

We recommend using the Linux remote synchronization and file transfer command [`rsync`](https://en.wikipedia.org/wiki/Rsync){:target="_blank"}. rsync uses an algorithm that minimizes the amount of data by moving only the portions of files that have changed; in addition, it supports compression.

We suggest using the following syntax:

	rsync -azvP <source> <destination>

Options:

	`a` archive
	`z` compress
	`v` verbose
	`P` partial progress

For additional options, see the [rsync man page](http://linux.die.net/man/1/rsync){:target="_blank"}.

To migrate static files:

1.	Log in to your local system in a terminal.
2.	Log in to your Magento Commerce (Cloud) account:

		magento-cloud login
3.	If necessary, change to the project directory.
4.	If necessary, check out the master branch:

		magento-cloud environment:checkout master
5.	Pull any changes from the server:

		git pull origin master
6.	Open an SSH connection to your Staging or Production environment:

			*	Staging: `ssh -A <project ID>_stg@<project ID>.ent.magento.cloud`
			*	Production: `ssh -A <project ID>@<project ID>.ent.magento.cloud`
7.	rsync the `pub/media` directory from your local Magento server to staging or production:

		rsync -azvP pub/media/ <developmemt machine user name>@<development machine host or IP>:pub/media/
	The IP is for the Magento Commerce VM or container you created when setting up the local.

## Migrate the database {#cloud-live-migrate-db}

**Prerequisite:** A database dump (see Step 3) should include database triggers. For dumping them, make sure you have the [TRIGGER privilege](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_trigger){:target="_blank"}.

**Important:** The Integration environment database is strictly for development testing and may include data you may not want to migrate into Staging and Production.

For continuous integration deployments, we **do not recommend** migrating data from Integration to Staging and Production. You could pass testing data or overwrite important data. Any vital configurations will be passed using the [configuration file]({{ page.baseurl }}cloud/live/sens-data-over.html) and `setup:upgrade` command during build and deploy.

We **do recommend** migrating data from Production into Staging to fully test your site and store(s) in a near-production environment with all services and settings.

To migrate a database:

1.	SSH into the environment you want to create a database dump from:

	*	Staging: `ssh -A <project ID>_stg@<project ID>.ent.magento.cloud`
	*	Production: `ssh -A <project ID>@<project ID>.ent.magento.cloud`
	* To SSH into the `master` branch of your Integration environment:

			magento-cloud environment:ssh
2.	Find the database login information:

		php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
3.	Create a database dump:

	For Starter environments and Pro Integration environments:

		mysqldump -h <database host> --user=<database user name> --password=<password> --single-transaction --triggers main | gzip - > /tmp/database.sql.gz

	For Pro Staging and Production environments, the name of the database is in the `MAGENTO_CLOUD_RELATIONSHIPS` variable (typically the same as the application name and user name):

		mysqldump -h <database host> --user=<database user name> --password=<password> --single-transaction --triggers <database name> | gzip - > /tmp/database.sql.gz

4.	Transfer the database dump to Staging or Production with an `rsync` command:

	*	Staging: `rsync -azvP /tmp/database.sql.gz <project ID>_stg@<project ID>.ent.magento.cloud:/tmp`
	*	Production: `rsync -azvP /tmp/database.sql.gz <project ID>@<project ID>.ent.magento.cloud:/tmp`
8.	Enter `exit` to terminate the SSH connection.
9.	Open an SSH connection to the environment you want to migrate the database into:

	*	Staging: `ssh -A <project ID>_stg@<project ID>.ent.magento.cloud`
	*	Production: `ssh -A <project ID>@<project ID>.ent.magento.cloud`
	* To SSH into the `master` branch of your Integration environment:

			magento-cloud environment:ssh
10.	Import the database dump:

		zcat database.sql.gz | mysql -u <username> -p<password> <database name>

	The following is an example using information from step 2:

		zcat database.sql.gz | mysql -u user main

### Troubleshooting the database migration
If you set up stored procedures or views in your database, the following error might display during the import:

	ERROR 1277 (42000) at line <number>: Access denied; you need (at least one of) the SUPER privilege(s) for this operation

The reason is that stored procedures and views both use `"DEFINER='root'@'localhost'"`, and you don't have `root` user access to the staging or production databases.

To solve the issue, create another database dump, replacing the `DEFINER` string with an empty string.

You can do this using a text editor or by using the following command:

	mysqldump -h <database host> --user=<database user name> --password=<password> --single-transaction main  | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | gzip > /tmp/database_no-definer.sql.gz

Use the database dump you just created to [migrate the database](#cloud-live-migrate-db).

<div class="bs-callout bs-callout-info" id="info">
  <p>After migrating the database, you can set up your stored procedures or views in Staging or Production the same way you did in your Integration environment.</p>
</div>

#### Next step
[Test deployment]({{ page.baseurl }}cloud/live/stage-prod-test.html)
