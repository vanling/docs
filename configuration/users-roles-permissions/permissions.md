---
description:
readTime:
---

# Permissions

> Intro

<!--
:::tip Before you Begin

Filler

:::

:::tip Learn More

Filler

:::
-->

## Configure Permissions

<video title="Configure Role Permissions" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

To configure a role's access permissions, follow these steps.

1. Navigate to **Settings > Roles & Permissions > [Role Name]**.
2. Scroll to **Permissions**. Each collection is a row. Its
   [CRUDS permissions](/configuration/users-roles-permissions.md#directus-permissions) are in columns.
3. Click the icon for the collection and permission type that you want to set. If you'd like to adjust permissions for
   Directus system collections, then click **System Collections** to expand the menu and access these collections.
4. Click the icon in the relevant collection row and CRUDS permission column and popup menu will appear with the
   following permission levels:
   - <span mi icon muted>check</span> **All Access** — Grants permission to all the collection's items.
   - <span mi icon muted>block</span> **No Access** — Denies permission to all the collection's items.
   - <span mi icon muted>rule</span> **Use Custom** — Grants partial permissions, based on configured conditions.
5. From here, there are two possibilities:
   - If you selected <span mi muted>check</span> **All Access** or <span mi muted>block</span> **No Access**, setup is
     complete.
   - If you chose <span mi icon muted>rule</span> **Use Custom**, please see
     [Configure Custom Permissions](#configure-custom-permissions).

::: warning Admin Roles

If you [configured the role's details](/configuration/users-roles-permissions/roles.md#configure-role-details) to have
**Admin Access**, permission configuration is disabled.

:::

## Configure Custom Permissions

<video title="Configure Role Permissions" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

To configure custom access permissions for a role, follow these steps.

1. Follow the steps to [configure permissions](#configure-permissions) and choose <span mi icon muted>rule</span> **Use
   Custom** on step four and a side drawer will open.
2. Configure custom access permission validations as desired.\
   **Note:** For details, please see the relevant sub-section below.
3. Click <span mi btn>check</span> in the side drawer header to confirm and save custom access permissions.

### Use Custom Create Permissions

<video title="Custom Create Permissions" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

- **Field Permissions** — Toggle to limit field access on item creation.
- **Field Validation** — Set [filters](/app/filters.md) to limit creation of items based on field value(s).
- **Field Presets** — Set default field values to create the item with.

### Use Custom Read Permissions

<video title="Custom Read Permissions" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

- **Item Permissions** — Set [filters](/app/filters.md) to define which items can be read.
- **Field Permissions** — Toggle to limit which fields can be read.

::: warning Read Field Permissions

Directus always requires read access to the Primary Key field (e.g., `id`) so it can uniquely identify items. Also, if a
collection has "Archive" or "Sort" fields configured, those fields will also need read access to use the App's
soft-delete and manual sorting features.

:::

### Use Custom Update Permissions

<video title="Custom Update Permissions" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

- **Item Permissions** — Set [filters](/app/filters.md) to define which items can be updated.
- **Field Permissions** — Toggle to limit which fields can be updated.
- **Field Validation** — Set [filters](/app/filters.md) to define which field values are valid in an update.
- **Field Presets** — Set default field values to update the item with.

### Use Custom Delete Permissions

<video title="Custom Delete Permissions" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

- **Item Permissions** — Set [filters](/app/filters.md) to define which items can be deleted.

### Use Custom Share Permissions

<video title="Custom Share Permissions" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

**Item Permissions** — Set [filters](/app/filters.md) to define which items can be shared.

## Toggle All Collection Permissions

<video title="Toggle all Collection Permissions" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

To grant or restrict all CRUDS permissions to a collection at once, follow these steps.

1. Navigate to **Settings > Roles & Permissions > [Role Name]**.
2. Mouse over the desired collection's name and the following options will appear:
   - **All** — Click to enable all CRUDS permissions for a collection.
   - **None** — Click to restrict all CRUDS permissions for a collection.

## Reset System Permissions

<video title="Reset System Permissions" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

This is only available when **App Access** is enabled when you
[configure role details](/configuration/users-roles-permissions/roles.md#configure-role-details).

1. Navigate to **Settings > Roles & Permissions > [Role Name]**.
2. Make sure [App Access](/configuration/users-roles-permissions/roles.md#configure-role-details) is enabled for the
   role.
3. At the bottom of **Permissions**, click **System Collections** to expand the menu and show system collections.
4. Scroll to the bottom and choose the following options, beside **Reset System Permissions to:**:
   - **App Access Minimum** — Reconfigures permissions on system collections to the bare minimum that are required to
     log in to the app.
   - **Recommended Defaults** — Reconfigures permissions on system collections to the recommended defaults.

The reconfigurations are made automatically and instantly.

:::tip

You may notice that when you toggle on **App Access Minimum** permissions will be hard-coded so they cannot be
restricted. However, you are free to reconfigure the **Recommended Defaults**.

:::
