---
description:
readTime:
---

# Users

> Intro

<!--
:::tip Before you Begin

Filler

:::

:::tip Learn More

Filler

:::
-->

<video title="Assign Role to User" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

Within the Data Studio, the [User Directory](/app/user-directory.md) is the primary place to configure a user's details.
However, you can also create or invite users and configure their role from **Settings > Roles & Permissions > [Role]**.

## Invite a User

<video title="Invite a User" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

1. Click <span mi btn muted>person_add</span> in the page header.
2. Enter one or more email addresses, separated by a comma and a space. Tip: You can also add emails on a new line.
3. Click **Invite** to confirm.

## Add an Existing User

<video title="Add an Existing User" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

1. Under the **Users in Role** section, click <span mi btn muted>playlist_add</span> and a drawer will open.
2. Select users as desired.
3. Click <span mi btn>check</span> to confirm and the drawer will close.\
   The added user(s) will now be visible under **Users in Role**.
4. Click <span mi btn>check</span> in the page header to confirm.

## Create a User

<video title="Create a User" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

1. Under the **Users in Role** section, click <span mi btn>add</span> and a drawer will open.
2. Fill in the user's details as desired.\
   The added user(s) will now be visible under **Users in Role**.
3. Click <span mi btn>check</span> to confirm and the drawer will close.

## Remove User from Role

<video title="Remove User from Role" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

1. Under the **Users in Role** section, click <span mi icon dngr>close</span> and the user will be removed from the
   role.

Once removed from a role, the user(s) will be given a `NULL` role until assigned a new role, limiting them to Public
permissions.

:::tip

You can also invite and create users, as well as manage their role from the [User Directory](/app/user-directory.md).

:::
