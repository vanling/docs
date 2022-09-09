---
description:
  Users are the individual accounts for authenticating into the API and App. Each user belongs to a Role which defines
  its access Permissions.
readTime: 7 min read
---

# Users, Roles & Permissions

> Users, roles, and permissions work together to determine _who can access what_ inside your database.
> [Users](/getting-started/glossary#users) are the individual accounts for authenticating into the API and App. Each
> user is assigned a [role](/getting-started/glossary#roles) which defines its access
> [access permissions](/getting-started/glossary#permissions).

![Users, Roles and Permissions](https://cdn.directus.io/docs/v9/configuration/users-roles-permissions/users-roles-permissions-20220909/users-roles-permissions-20220907A.webp)

In order to understand how users, roles, and permissions work in Directus, you'll need a conceptual understanding of
_how they work in general_. The following few paragraphs will introduce you to the concepts of users, roles, and
permissions. If you're already familiar, feel free to skip over the following paragraphs and see
[How it Works in Directus](#how-it-works-in-directus).

:::tip Before You Begin

We recommend you try the [Quickstart Guide](/getting-started/quickstart.md) to get an overview of the platform.

:::

:::tip Learn More

To manage users, role and permissions programmatically via the API, please see our API guides on
[users](/reference/system/users.md), [roles](/reference/system/roles.md), and
[permissions](/reference/system/permissions.md).

:::

### Users

Projects typically have many different kinds of users. For example, you'll need developers and administrators to design
the data model and manage all its data. Your team may have other users, such as data analysts, content writers, or
managers who need to access to sensitive data. Finally, you may also have end-users, such as customers, subscribers, 3rd
party sellers, and beyond who typically need more limited access to the database. The kinds of data that your team and
end-users will need completely depend on your project.

Remember, users are simply rows of information in data tables. It can be easy to forget this for people that are new to
relational data models, as the term _users_ may create a personification in our minds which distinguishes or elevates
the term from _"data"_. But this is not the case; from the perspective of the data model, users are data. Another key
point is that a user does not need to be a person at all. A user could be an AI bot, chat bot, API, or any other entity
that can login and interact with the database.

### Permissions

For the majority of projects, it wouldn't be safe or ethical to give every user full access to the database. Users could
accidentally damage data or even take malicious actions against the project and its users. So to help prevent this,
databases let you create access permissions which define what each user can do to the data in each data table.

There are four types of permissions, based on the four actions you can do to data. That is, you can _create, read,
update, or delete_ data... Hence you may often hear people use the term CRUD permissions. You can configure CRUD
permissions on each data table as desired. For example, you can grant only read permissions on a data table, read and
write but not update or delete permissions, _or any other combination of the four_.

In many cases you will need users to grant permissions to some but not all of the data in a data table. So to do this,
SQL databases allow us to create business rules, which let us grant access to data conditionally. For example, students
may need access to their own grades, but not the grades of other students. This is achieved with a business rule.

We'll discuss business rules more in [customizing permissions](#customizing-permissions) below, but first we need an
introduction to roles.

### Roles

It would be a tedious, complicated, and error-prone process to configure and assign permissions individually to each and
every user. In many cases, your project will have multiple users doing the same thing _(managers, writers, subscribers,
etc)_. If we assigned permissions directly, we would have to do the same work over and over, which leads to a higher
chance of misconfiguring permissions. This is a common type of problem called
[data duplication](/configuration/data-model.md#avoid-data-duplication). To avoid this problem, we create roles,
configure the role's permissions once, then assign the role to users as desired.

Regardless of your project, your SQL database will _always_ need an admin role and a public role. In addition, you may
need any number of custom roles.

**Administrators**\
The administrator role provides complete, unrestricted control over the database, including the data model and all it data.
This cannot be limited, as by definition it would no longer be an administrator role. You need at least one user in an administrator
role. Otherwise, it would be impossible to fully manage the database.

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

<!-- ![Roles](image.webp) -->

Remember, in many cases you'll want to use business rules to assign more granular permissions to roles, granting access
to _some but not all_ of the data in a data table.

To give one example of a business rule, we could create a data table `test_a_answers`, which stores student answers to
"Test A". Then we could enable read and write permissions for the student role, but not update or delete permissions.
That way, students can take the test, but they can't change their answers or delete their test and start over.

However, at this point, students would still have access to _all the tests_ in that table, including those of their
classmates, which makes it possible to read _(or even write!)_ their classmates' test answers. So we would need business
rules in order to:

- limit permissions so students can only create and read their own test answers.
- disable permissions after a certain point in time, creating a deadline.

### Working with Users, Roles and Permissions

<!-- ![Working with Users, Roles and Permissions](image.webp) -->

Within an SQL database- users, roles, and permissions are created and managed using the SQL language. While you have
full reign to configure these using SQL, Directus also provides a complete system to configure and manage users, roles,
and permissions without writing a single line of SQL.

## How it Works in Directus

<video title="How Users, Roles, & Permissions Work" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/docs/v9/configuration/users-roles-permissions/users-roles-permissions-20220909/how-users-roles-and-permissions-work-20220909A.mp4" type="video/mp4" />
</video>

Directus enables you to create as many roles as needed, configure granular permissions, and assign roles to users as
desired. To create a new role, follow these steps.

1. [Create a Role](#create-a-role).
2. [Configure its Permissions](#configure-role-permissions).
3. [Assign Role to User](#assign-role-to-user).

:::tip

Remember, the following users, role and permissions systems built into Directus cannot be deleted, however using them is
optional. You may configure your own system as desired.

:::

## Directus Users

![Users in the Directus Data Studio](https://cdn.directus.io/docs/v9/configuration/users-roles-permissions/users-roles-permissions-20220909/users-20220807A.webp)

Within the Data Studio, users are managed within the [User Directory](/app/user-directory.md). However, there are some
controls available to assign users to roles in **Settings > Roles and Permissions**.

To learn more, please see our guide on [users](/configuration/users-roles-permissions/users.md).

## Directus Roles

![Roles in the Directus Data Studio](https://cdn.directus.io/docs/v9/configuration/users-roles-permissions/users-roles-permissions-20220909/roles-20220907A.webp)

Within the Data Studio, roles are configured in **Settings > Roles and Permissions**. You can create as many roles as
you need for your project. Directus also comes with built-in administrator and public roles, which cannot be deleted.

The administrator role provides full permissions for all data in the app, and this cannot be limited, as by definition
it would no longer be an admin role. You must always have at least one user with an administrator role.

The public role comes with all access permissions turned off by default, but this can be reconfigured as desired.
Remember, access permissions granted to this role apply to everyone, including unauthenticated web traffic _and all
existing users_. If you wish to keep the project private, simply keep all permissions turned off.

To learn more, see our guide on [roles](/configuration/users-roles-permissions/roles.md).

## Directus Permissions

![Roles in the Directus Data Studio](https://cdn.directus.io/docs/v9/configuration/users-roles-permissions/users-roles-permissions-20220909/permissions-20220907A.webp)

Within the Data Studio, permissions are configured in **Settings > Roles and Permissions**. Directus offers an extremely
granular, yet easy to configure permissions system. When you [create a role](#create-a-role), all permissions are turned
off by default, allowing you to explicitly grant permissions as desired.

In addition, Directus has two differences compared to working with a typical SQL database permissions system. First, the
term [custom access permissions](/configuration/users-roles-permissions/permissions.md#configure-custom-permissions) is
used in place of [business rules](#customizing-permissions), however the concept is the same. Second, instead of the
standard CRUD permissions, Directus provides CRUDS permissions: _create, read, update, delete, and share_. This _fifth_
type of permission, share, defines whether a user has permissions to perform [data sharing](/app/content/shares.md) on
items in a collection.

To learn more, see our guide on [permissions](/configuration/users-roles-permissions/permissions.md)

## Workflows

![Workflows in the Directus](https://cdn.directus.io/docs/v9/configuration/users-roles-permissions/workflows-20220909/workflows-20220909B.webp)

Workflows are a way to setup structured stages to content authoring and data management. They are created primarily with
custom access permissions, but can be enhanced with email notifications, custom interfaces, and
[flows](/configuration/flows.md). Directus supports endlessly configurable workflows.

To learn more, see our guide on [Workflows](/configuration/users-roles-permissions/workflows.md).
