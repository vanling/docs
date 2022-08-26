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

If you're familiar with users, roles, and permissions in SQL, feel free to skip over the following paragraphs and see
[How it Works](#how-it-works) in Directus.

Projects typically have many different kinds of users. For example, you'll need developers and administrators to design
the data model and manage all its data. Your team may have other business users, such as data analysts, content writers,
managers who need to access and manage sensitive data. Finally, you may also have customers, subscribers, 3rd party
sellers, students, teachers, and beyond. The types of users you will need completely depends on your project.

It wouldn't be safe or ethical for every user to have full access of all the data and data tables in the database.
People might accidentally damage data or even take malicious actions against the project and its users. So to prevent
this, databases let you assign access permissions to each user. Access permissions define what a user can do inside a
database. That is to say, they determine whether a user can create, read, update, or delete data within a given data
table.

Instead of configuring and assigning access permissions directly to each individual user, you create a role and assign
access permissions to the role. A role is simply a group of pre-configured access permissions that you can assign to
multiple users. Here are two kinds of roles you will always need:

**Administrators**\
Users who design and manage the data model and all its data. They have full access to the database. Every database _must_
have at least one user in an administrator role. Otherwise, it would be impossible to fully manage the database. An administrator
role cannot have limited access permissions, as by definition, _it would no longer be an administrator role_.

**Public**\
A public role defines access permissions for unauthenticated web traffic. Therefore it has the most limited access to the
database. For the public role, the database has no idea which data you'd want the public to see, and in fact, in some projects
you will want to keep data completely private, only accessible to logged-in, authenticated users. So to be safe, all permissions
begin turned off. It is up to the administrators to re-configure this and define exactly what a random, unauthenticated user
has access to within the database.

In addition to these two extreme types of roles, Admins which has full permissions and public which defines minimum
permissions, you may need to create more roles. The roles you decide to create and the permissions you assign to each
role are completely open-ended and dependent on your project's needs.

Typically, users, roles and permissions are created using SQL. While you have full reign to configure these using SQL,
the Directus Data Studio enables you to configure these in a GUI, without writing a single line of SQL.

## How it Works

<video title="How Roles & Permissions Work" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

Directus comes with built-in Admin and Public roles, which cannot be deleted. The Admin role provides full Permissions
for all data in the app, and this cannot be limited. The Public role comes with all access permissions turned off by
default and these can be fully reconfigured as needed. This Public role determines the access permissions given for any
unauthenticated request to app data including unauthenticated users, visitors to your website or any other web request
to your Directus Project's API.

Administrators can create roles, configure permissions, and assign roles to users as desired. It is a fairly simple
three step process.

1. [Create a Role](#create-a-role)
2. [Configure Role Permissions](#configure-role-permissions)
3. [Assign Role to User](#assign-role-to-user)

<!--

Roles with _App Access_ enabled are created with some limited Permissions configured by default, so they can access the app and their own profile information.
Roles that have neither _Admin_ nor _App Access_ enabled (such as the built-in _Public_ Role) are created with Public access permissions.
### Configure Public Permissions

The Public permissions control what project data is accessible without authentication. This is managed via the Public
"role", which is included in the system by default and can not be deleted.

::: warning Private by Default

All of the data within the platform is private by default. Permissions for the public role can be granted on a
case-by-case basis by administrators.

:::

-->

## Create a Role

<video title="Create a Role" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

To create a role, follow these steps.

1. Navigate to **Settings > Roles & Permissions**.
2. Click <span mi btn>add</span> in the page header.
3. Enter a unique **Role Name**.
4. Toggle **App Access** and **Admin Access** as desired:
   - **App Access** — Configures minimum access permissions required to log in to the App.
   - **Admin Access** — Configures full access permissions to all project data and settings.
5. Click on **Save** to confirm.

:::tip

Next, you will likely need to [configure the role's details](#configure-role-details) and
[configure the role's permissions](#configure-role-permissions).

:::

## Configure Role Permissions

<video title="Configure Role Permissions" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

Directus possesses an extremely granular, yet easy to configure permissions system. When you
[create a role](#create-a-role), all permissions are turned off by default. This allows you to give explicit access to
only what is required. Individual permissions are applied to the role, and each is scoped to a specific collection and
CRUD action (create, read, update, delete).

:::tip Saves Automatically

Every reconfiguration made to permissions is saved automatically and instantly.

:::

::: warning Admin Roles

If you [configure the role's details](#configure-role-details) to have **Admin Access**, Permission configuration is
disabled as the role has complete access to the platform.

:::

1. Navigate to **Settings <span mi icon dark>chevron_right</span> Roles & Permissions
   <span mi icon dark>chevron_right</span> [Role Name]**
2. Scroll to the **Permissions** section
3. **Click the icon** for the collection (row) and action (column) you want to set
4. Choose the desired permission level: <span mi icon>check</span> **All Access**, <span mi icon>block</span> **No
   Access**, or <span mi icon>rule</span> **Use Custom**

If you selected **"<span mi icon>check</span> All Access"** or **"<span mi icon>block</span> No Access"** then setup is
complete. If you chose to customize permissions then continue with the appropriate guide below based on the relevant
_action_.

<!-- CRUDS + Permission Inheritance -->
<!-- Shares, Flows, ??? use Permission Inheritance (perhaps this should go in glossary) -->

### Create (Custom Access)

5. **Field Permissions** control which fields accept a value on create. Fields are individually toggled.
6. **Field Validation** define the rules for field values on create
7. **Field Presets** control the field defaults when creating an item

### Read (Custom Access)

5. **Item Permissions** control which items can be read, as defined by the [Filter Rules](/reference/filter-rules)
   entered.
6. **Field Permissions** control which fields can be read. Fields are individually toggled.

::: warning Read Field Permissions

The Directus App always requires read access to the Primary Key field (e.g., `id`) so it can uniquely identify items.
Also, if a Collection has "Archive" or "Sort" fields configured, those fields will also need read access to use the
App's soft-delete and manual sorting features.

:::

### Update (Custom Access)

5. **Item Permissions** control which items can be updated, as defined by [Filter Rules](/reference/filter-rules).
6. **Field Permissions** control which fields can be updated. Fields are individually toggled.
7. **Field Validation** define the rules for field values on update, as defined by
   [Filter Rules](/reference/filter-rules).
8. **Field Presets** control the field defaults when updating an item

### Delete (Custom Access)

5. **Item Permissions** control which items can be deleted, as defined by the [Filter Rules](/reference/filter-rules)
   entered.

---

### Configure System Permissions

In addition to permissions for _your_ custom collections, you can also customize the permissions for _system_
collections. To edit system permissions, follow these steps:

1. Go to **Roles and Permissions** and select the desired Role.\
   _You will be taken to the permissions configuration page._
2. Click **System Collections** at the bottom of the page.
3. Find the desired System Collection.
4. Set permissions as desired.

There are two pre-configured options you can use for resetting the role's system permissions and ensure proper App
access. To access these, click "System Collections" to expand, and then click one of the buttons at the bottom of the
listing.

- **App Access Minimum** — The minimum permissions required to properly access the App
- **Recommended Defaults** — More permissive but balanced for a better App user experience

:::tip Remember

When App Access is enabled, Directus will automatically add _(and hardcode)_ permissions for the necessary system
collections for that Role.

:::

## Reset Permissions

<video title="Reset Permissions" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

Sometimes, you may need to reset a role's permissions on or off. There are several ways to do this. First, you could
manually reconfigure each CRUDS permission, as shown in the [configure role permissions](#configure-role-permissions)
section. Additionally, you have the option to

- Toggle All/None for a specific row.
- Revert to **App Access Minimum** OR **Recommended Defaults**

## Configure Role Details

![Configure Role Details](image.webp)

- **Permissions** — Configure [access permissions](#configure-permissions) for the role.
- **Role Name** — Sets the name of the role.
- **Role Icon** — Sets icon used throughout the App when referencing this role.
- **Description** — Adds a note to help explain the role's purpose.
- **App Access** — Auto-configures minimum permissions required to log in to the App.
- **Admin Access** — Auto-configures full permissions to project data and Settings.
- **IP Access** — Sets an IP address whitelist. Leave empty to allow all IP addresses.
- **Require MFA** — Forces all users within this role to use two-factor authentication.
- **Users in Role** — Lists all users within this role.

## Manage Users within a Role

<video title="Assign Role to User" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

You can invite and create users, as well as manage their role from the [User Directory](/app/user-directory.md). To
assign a role to a user from within **Settings > Roles & Permissions > [Role]**, follow these steps.

### Invite a User

1. Click <span mi btn muted>person_add</span> in the page header.
2. Enter one or more email addresses, separated by a comma and a space. Tip: You can also add emails on a new line.
3. Click **Invite** to confirm.

### Add an Existing User

1. Under the **Users in Role** section, click <span mi btn muted>playlist_add</span> and a drawer will open.
2. Select users as desired.
3. Click <span mi btn>check</span> to confirm and the drawer will close.\
   The added user(s) will now be visible under **Users in Role**.
4. Click <span mi btn>check</span> in the page header to confirm.

### Create a User

1. Under the **Users in Role** section, click <span mi btn>add</span> and a drawer will open.
2. Fill in the user's details as desired.\
   The added user(s) will now be visible under **Users in Role**.
3. Click <span mi btn>check</span> to confirm and the drawer will close.

### Remove User from Role

1. Under the **Users in Role** section, click <span mi icon dngr>close</span> and the user will be removed from the
   role.

Until assigned a new role, the user(s) will be given a `NULL` role, which limits them to Public permissions.

## Configure Workflows

<video title="Configure Workflows" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

Workflows are a way to add structured stages to the flow of content authoring. They are primarily defined through the
permissions for a Collection, but can be further enhanced via email notifications, custom interfaces, and automation.
Directus supports endlessly configurable workflows, so we will only cover one simple example below.

1. To create a structured workflow for **Articles**, the first step is
   [Creating a Field](/configuration/data-model#creating-a-field) to track the article "status" — we'll call it
   **Status**, but it can be named anything.
2. Next, create different Roles for each stage of the workflow, such as `author` and `manager`.
3. Finally, configure the Role permissions based on the possible values of that Status field, such as `draft`, `review`,
   `approved`, and `published`, so that they are properly restricted to create content and update the status.
   - The Author can create content, but only save a status of `draft` or `review`.
   - The Manager has additional permissions that allow them to save statuses of `approved` or `published`.

## Delete a Role

<video title="Create a Role" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

1. Navigate to **Settings <span mi icon dark>chevron_right</span> Roles & Permissions
   <span mi icon dark>chevron_right</span> [Role Name]**.
2. Click <span mi btn dngr>delete</span> in the page header and a popup will appear.
3. Click **Delete** to confirm.

::: warning Users in a Deleted Role

If you delete a role that still has users in it, those users will be given a `NULL` role, limits them to Public
permissions. From here, you can [assign them to a new role](#add-an-existing-user).

:::

::: warning Last Admin

You must maintain at least one user in an administrator role in order to manage the project.

:::

::: warning Public Role

The core platform does not allow you to delete the Public role. This is because there must to be a defined behavior for
when an unauthenticated user tries to access the platform. If you'd like to disable public access, simply turn off all
permissions.

:::
