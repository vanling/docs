---
description:
  Users are the individual accounts for authenticating into the API and App. Each user belongs to a Role which defines
  its access Permissions.
readTime: 7 min read
---

# Users, Roles & Permissions

> [Users](/getting-started/glossary#users) are the individual accounts for authenticating into the API and App. Each
> user belongs to a [Role](/getting-started/glossary#roles) which defines its access
> [Permissions](/getting-started/glossary#permissions).

:::tip Before You Begin

We recommend you try the [Quickstart Guide](/getting-started/quickstart.md) to get an overview of the platform.

:::

:::tip Learn More

To manage users, role and permissions programmatically via the API, please see our API guides on
[users](/reference/system/users.md), [roles](/reference/system/roles.md), and
[permissions](/reference/system/permissions.md).

:::

In order to understand how users, roles, and permissions work in Directus, you'll need a conceptual understanding of
_how they work in general_. The following few paragraphs will introduce you to these concepts of users, roles, and
permissions. If you're already familiar, feel free to skip over the following paragraphs and see
[How it Works in Directus](#how-it-works-in-directus).

### Users

Projects typically have many different kinds of users. For example, you'll need developers and administrators to design
the data model and manage all its data. Your team may have other business users, such as data analysts, content writers,
managers who need to access and manage sensitive data. Finally, you may also have customers, subscribers, 3rd party
sellers, students, teachers, and beyond. The types of users you will need completely depends on your project.

### Permissions

It wouldn't be safe or ethical for every user to have full access of all the data and data tables in the database.
People might accidentally damage data or even take malicious actions against the project and its users. So to prevent
this, databases let you assign access permissions to each user. Access permissions define what a user can do inside a
database. That is to say, they determine whether a user can create, read, update, or delete data within a given data
table.

### Roles

Instead of configuring and assigning access permissions directly to each individual user, you create a role and assign
access permissions to the role. A role is simply a group of pre-configured access permissions that you can assign to
multiple users. Here are two kinds of roles you will always need:

**Administrators**\
The administrator roles is for users that design and manage the data model and all its data. They have full access to the
database. Every database _must_ have at least one user in an administrator role. Otherwise, it would be impossible to fully
manage the database. An administrator role cannot have limited access permissions, as by definition, _it would no longer
be an administrator role_.

**Public**\
A public role defines access permissions for unauthenticated web traffic. Therefore it has the most limited access to the
database. For the public role, the database has no idea which data you'd want the public to see, and in fact, in some projects
you will want to keep data completely private, only accessible to logged-in, authenticated users. So to be safe, all permissions
begin turned off. It is up to the administrators to re-configure this and define exactly what a random, unauthenticated user
has access to within the database.

In addition to these two extreme types of roles, you may need to create more roles each with their own unique set of
permissions. The roles you decide to create and the permissions you assign to each role are completely open-ended and
dependent on your project's needs.

### Working with Users, Roles and Permissions

Typically, users, roles and permissions are created using SQL. While you have full reign to configure these using SQL,
the Directus Data Studio enables you to configure these in a GUI, without writing a single line of SQL.

<!--
Many other systems provide a GUI to toggle *some* data.
For example, LMS, students don't have access -> teachers can "release content"!
However, the vast majority of projects are limited by design.
In Directs, you can configure permissions to any type of role.
-->

## How it Works in Directus

<video title="How Roles & Permissions Work" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

Directus comes with built-in Admin and Public roles, which cannot be deleted. The Admin role provides full Permissions
for all data in the app, and this cannot be limited. The Public role comes with all access permissions turned off by
default and these can be fully reconfigured as needed. This Public role determines the access permissions given for any
unauthenticated request to app data including unauthenticated users, visitors to your website or any other web request
to your Directus Project's API.

Administrators can create roles, configure permissions, and assign roles to users as desired. It is a fairly simple
process.

1. [Create a Role](#create-a-role).
2. [Configure Role Permissions](#configure-role-permissions).
3. [Assign Role to User](#assign-role-to-user).

<video title="Directus Roles" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

## Permissions

<video title="Permissions" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

## Users

<video title="Directus Users" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

## Configure Workflows

<video title="Configure Workflows" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

Workflows are a way to add structured stages to the flow of content authoring. They are primarily defined through the
permissions for a Collection, but can be further enhanced via email notifications, custom interfaces, and automation.
Directus supports endlessly configurable workflows. To learn more, see our documentation on
[Workflows](/configuration/users-roles-permissions/workflows.md).

<!--
- workflows
- notifications
- data flows + github/netlify?
-->
