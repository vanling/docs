# Roles

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

Next, you will likely need to [configure the role's permissions](#configure-role-permissions) and
[configure the role's details](#configure-role-details).

:::

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
