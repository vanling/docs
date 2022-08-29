# Permissions

## Configure Permissions

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

1. Navigate to **Settings > Roles & Permissions > [Role Name]**.
2. Scroll to the **Permissions** section.
3. **Click the icon** for the collection (row) and action (column) you want to set.
4. Choose the desired permission level: <span mi icon>check</span> **All Access**, <span mi icon>block</span> **No
   Access**, or <span mi icon>rule</span> **Use Custom**.

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

### Toggle all of a Collection's Permissions

You have the option to toggle permissions to All/None for a specific collection, revert to app access minimum, revert
recommended defaults.

1. Navigate to **Settings > Roles & Permissions > [Role Name]**.
2. Mouse over the desired collection and click:
   - **All** to enable all CRUDS permissions for a collection.
   - **None** to restrict all CRUDS permissions for a collection.

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

### Reset System Permissions

This is only available when **App Access** is enabled under [role details](#configure-role-details).

1. Navigate to **Settings > Roles & Permissions > [Role Name]**.
2. At the bottom of **Permissions**, click **System Collections** to expand the menu and show System Collections.
3. Scroll to the bottom. Under **Reset System Permissions to:** choose the following:
   - **App Access Minimum** — Reconfigures permissions on system collections to the bare minimum required to log in to
     the app.
   - **Recommended Defaults** — Reconfigures permissions on system collections to the recommended defaults.
