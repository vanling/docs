---
description:
readTime:
---

# Users

>

<!--
:::tip Before you Begin

Filler

:::

:::tip Learn More



:::
-->

Within the Data Studio, the [User Directory](/app/user-directory.md) is the primary place to manage users. However,
**Settings > Roles & Permissions > [Role]** includes controls to manage a user's role as well.

## Invite a User

<video title="Invite a User" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

Directus lets you invite users to your Directus project via email. When you do this within **Settings > Roles &
Permissions > [Role]**, the role will be assigned to the user when they accept invitation and register. To invite a
user, follow these steps.

1. Navigate to **Settings > Roles & Permissions > [Role]**.
2. Click <span mi btn muted>person_add</span> in the page header.
3. Enter one or more email addresses, separated by a comma and a space.
4. Click **Invite** to confirm.

:::tip

Instead of comma-separated emails, you can also add emails on a new line.

:::

## Assign Role to Existing User

<video title="Add an Existing User" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

To add an existing user to the role list, follow these steps.

1. Navigate to **Settings > Roles & Permissions > [Role]**.
2. Under the **Users in Role** section, click <span mi btn muted>playlist_add</span> and a drawer will open.
3. Select users as desired.
4. Click <span mi btn>check</span> to confirm and the drawer will close.\
   The added user(s) will now be visible under **Users in Role**.
5. Click <span mi btn>check</span> in the page header to confirm.

## Create a User

<video title="Create a User" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

To create a user and assign their role, follow these steps.

1. Navigate to **Settings > Roles & Permissions > [Role]**.
2. Under the **Users in Role** section, click <span mi btn>add</span> and a drawer will open.
3. Fill in the user's details as desired.\
   The newly created user will now be visible under **Users in Role**.
4. Click <span mi btn>check</span> to confirm and the drawer will close.

## Remove User's Role

<video title="Remove User from Role" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

To remove a user from a role, follow these steps.

1. Under the **Users in Role** section, click <span mi icon dngr>close</span> and the user will be removed from the
   role.

Once removed from a role, the user(s) will be given a `NULL` role until assigned a new role, limiting them to Public
permissions.

:::tip

You can also invite and create users, as well as manage their role from the [User Directory](/app/user-directory.md).

:::
