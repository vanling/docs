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

1. Navigate to **Settings > Roles & Permissions > [Role Name]**.
2. Scroll to the **Permissions** section. Notice the each collection is in a row and its CRUDS permissions are in
   columns.
3. Click the icon for the collection and permission type that you want to set. If you'd like to adjust permissions for
   Directus system collections, then click **System Collections** to expand the menu and access these collections.
4. Click the icon in the relevant collection row and CRUDS permission column and popup menu will appear with the
   following permission levels:
   - <span mi icon muted>check</span> **All Access** — Grants the role permission to all the collection's items.
   - <span mi icon muted>block</span> **No Access** — Denies the role permission to all the collection's items.
   - <span mi icon muted>rule</span> **Use Custom** — Grants the role permissions to some items and fields of items.
5. Click to reconfigure the permissions level as desired.

Every reconfiguration is saved automatically and instantly. If you selected <span mi muted>check</span> **All Access**
or <span mi muted>block</span> **No Access** then setup is complete. If you chose <span mi icon muted>rule</span> **Use
Custom**, a drawer will open.

5. Configure custom access permission validations as desired. To learn more, please read the relevant section below.
6. Click <span mi btn>check</span> in the side drawer header to confirm and save custom access permissions.

::: warning Admin Roles

If you [configured the role's details](/configuration/users-roles-permissions/roles.md#configure-role-details) to have
**Admin Access**, permission configuration is disabled.

:::

## Custom Create Permissions

<video title="Configure Role Permissions" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

Custom create permissions is broken into the following three sub-menus, shown in the drawer's nav bar on the left:

- **Field Permissions** — Toggle a field to grant or prevent access when creating an item.
- **Field Validation** — Set [filters](/app/filters.md) to define valid field values when creating an item.
- **Field Presets** — Set default field values to be applied when creating an item.

## Custom Read Permissions

- **Item Permissions** — Set [filters](/app/filters.md) to define which items can be read.
- **Field Permissions** control which fields can be read. Fields are individually toggled.

::: warning Read Field Permissions

The Directus App always requires read access to the Primary Key field (e.g., `id`) so it can uniquely identify items.
Also, if a Collection has "Archive" or "Sort" fields configured, those fields will also need read access to use the
App's soft-delete and manual sorting features.

:::

## Custom Update Permissions

5. **Item Permissions** control which items can be updated, as defined by [Filter Rules](/reference/filter-rules).
6. **Field Permissions** control which fields can be updated. Fields are individually toggled.
7. **Field Validation** define the rules for field values on update, as defined by
   [Filter Rules](/reference/filter-rules).
8. **Field Presets** control the field defaults when updating an item

## Custom Delete Permissions

5. **Item Permissions** control which items can be deleted, as defined by the [Filter Rules](/reference/filter-rules)
   entered.

## Custom Share Permissions

Remember, [shares](/app/content/shares.md) let you grant access to an item in a collection to people that would
otherwise not have access permissions.

**Item Permissions**

## Toggle All Collection Permissions

You have the option to toggle permissions to All/None for a specific collection, revert to app access minimum, revert
recommended defaults.

1. Navigate to **Settings > Roles & Permissions > [Role Name]**.
2. Mouse over the desired collection and click:
   - **All** to enable all CRUDS permissions for a collection.
   - **None** to restrict all CRUDS permissions for a collection.

## Reset System Permissions

This is only available when **App Access** is enabled under [role details](#configure-role-details).

1. Navigate to **Settings > Roles & Permissions > [Role Name]**.
2. At the bottom of **Permissions**, click **System Collections** to expand the menu and show System Collections.
3. Scroll to the bottom. Under **Reset System Permissions to:** choose the following:
   - **App Access Minimum** — Reconfigures permissions on system collections to the bare minimum required to log in to
     the app.
   - **Recommended Defaults** — Reconfigures permissions on system collections to the recommended defaults.
