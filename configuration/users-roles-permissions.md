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

## Users, Roles & Permissions

In order to understand how users, roles, and permissions work in Directus, you'll need a conceptual understanding of
_how they work in general_. The following few paragraphs will introduce you to the concepts of users, roles, and
permissions. If you're already familiar, feel free to skip over the following paragraphs and see
[How it Works in Directus](#how-it-works-in-directus).

### Users

Projects typically have many different kinds of users. For example, you'll need developers and administrators to design
the data model and manage all its data. Your team may have other users, such as data analysts, content writers, or
managers who need to access to sensitive data. Finally, you may also have end-users, such as customers, subscribers, 3rd
party sellers, and beyond who typically need more limited access to the database. The kinds of data that your team and
end-users will need completely depend on your project.

### Permissions

Of course, for the majority of projects, it wouldn't be safe or ethical to give every user full access to the database.
People might accidentally damage data or even take malicious actions against the project and its users. So to prevent
this, databases let you assign access permissions to its users. Access permissions define what a user can do inside a
database. That is, they determine which data a user can create, read, update, or delete.

### Roles

It could be tedious and complicated if we assigned permissions individually to each and every user. Plus when have two
content writers, subscrbers, etc.. there would be a higher chance of misconfiguring their permissions. So instead, you
use roles to store permissions. Once you create a role, you can configure its permissions and assign it to users as
desired.

First, regardless of your project, you will _always_ need an admin role and a public role.

**Administrators**\
The administrator role is for users that design and manage the data model and all its data. Admins have unrestricted access
to the database and this cannot be limited. They manage the data model and all data, define roles, configure permissions,
and beyond. Every database _must_ have at least one user in an administrator role. Otherwise, it would be impossible to fully
manage the database.

**Public**\
A public role defines access permissions for unauthenticated requests to the database. That means that if you enable an access
permission for this role, _everybody has that permission enabled_. Remember, the database has no idea which data you'd want
the public to see. So to be safe, all permissions begin turned off by default. It is up to the administrators to re-configure
these and define exactly what the public role has access to.

**Custom Roles**\
In addition to these two extreme types of roles, you may need to create more roles each with their own unique set of permissions.
The roles you create and the permissions you configure for them are completely open-ended and dependent on your project's
needs.

### Customizing Permissions

![Roles](image.webp)

Remember, there are four operations you can perform on data in a database: _create, read, update and delete_. Therefore,
you can give a role the permissions to create and read data from a data table but not update or delete it, _or
vice-versa, or any other combination of the four_. Taking this one step further, you can even grant a permission based
on another value within the database. This kind of permission is often referred to as a _business rule_. This level of
control is important in the vast majority of projects.

To give one example, we could create a data table `test_a_answers`, which stores student answers to `Test A`. Then we
could enable read and write permissions for the student role, but not update or delete permissions. That way, students
can take the test, but they can't change their answers or delete their test and start over.

However, at this point, students would still have access to _all the tests_ in that table, including those of their
classmates, which makes it possible to read _(or even write!)_ their classmates' test answers. So we would want to
create business rules in order to:

- limit permissions so students can only create and read their own test answers.
- disable permissions after a certain point in time, creating a deadline.

### Working with Users, Roles and Permissions

![Working with Users, Roles and Permissions](image.webp)

For SQL databases, users, roles and permissions are created using the SQL language. While you have full reign to
configure these using SQL, Directus also provides a complete system to configure and manage users, roles, and
permissions without writing a single line of SQL.

## How it Works in Directus

<video title="How Users, Roles, & Permissions Work" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

No matter your data model or project requirements, Directus enables you to create as many roles as needed, configure
granular permissions, and assign roles to users as desired. To create a new role, follow these steps.

1. [Create a Role](#create-a-role).
2. [Configure its Permissions](#configure-role-permissions).
3. [Assign Role to User](#assign-role-to-user).

## Directus Users

<video title="Directus Users" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

Directus comes with a pre-configured user collection. You cannot change the existing fields and you cannot delete this
collection. However, you can add additional fields as desired, or even create your own custom `users` collection
entirely. Directus users are managed within the User Directory. However, there are some controls available to assign
users to roles within **Settings > Roles and Permissions**. To learn more, please see our guide on
[users](/configuration/users-roles-permissions/users.md).

## Directus Roles

<video title="Directus Roles" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

Directus roles are composed of pre-configured permissions for each collection, as well as a number of other role
details. You can create as many roles as you need for your project.

The platform comes with built-in administrator and public roles, which cannot be deleted. The Admin role provides full
permissions for all data in the app, and this cannot be limited. The public role comes with all access permissions
turned off by default, however these can be fully reconfigured as needed. This public role determines the access
permissions given for any unauthenticated request to app data including unauthenticated users, visitors to your website
or any other web request to your Directus Project's API. To learn more, see our guide on
[roles](/configuration/users-roles-permissions/roles.md).

## Directus Permissions

<video title="Directus Permissions" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

Directus offers a granular permissions system. However, in Directus, there are two differences compared to working with
a typical SQL database permissions system. First, we replaced term _business rules_ with **custom access permissions**,
however this functionality is the same. Second, instead of the standard CRUD permissions, Directus has CRUDS
permissions: _create, read, update, delete, shares_. This _fifth_ type of permission defines whether a user has
permissions to perform [data sharing](/app/content/shares.md) on items in a collection.

## Workflows

<video title="Configure Workflows" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

Workflows are a way to setup structured stages to content authoring and data management. They are created primarily with
custom access permissions, but can be enhanced with email notifications, custom interfaces, and
[flows](/configuration/flows.md). Directus supports endlessly configurable workflows. To learn more, see our
documentation on [Workflows](/configuration/users-roles-permissions/workflows.md).
