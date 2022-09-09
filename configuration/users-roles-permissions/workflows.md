# Workflows

> Workflows are a way to add structured stages to the flow of content authoring. This enables one to do things like pass
> off work in progress between multiple roles, create multi-signature approvals, build decision trees, _and beyond!_

A workflow is defined with custom access permissions. Basically, you configure each role's custom access permissions on
some collection(s), defining what they can and can't have access to, so that the each role's CRUDS permissions change at
each stage in the workflow.

<!-- <video title="Configure Workflows" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video> -->

There are an infinite number of possible workflows you could configure. However, to give an example, we will configure a
simple workflow for an `articles` collection. In our workflow, the workflow logic will be managed via the `status`
field. Someone from the `author` role will create an article, then pass it to an `editor` role who will co-edit the
article. Once finished, the `editor` will publish the document and handle future updates.

![A Workflow](/configuration/users-roles-permissions/work-flows.jpg)

| Draft                                                                     | Under Review                                                                         | Published                                                                                 |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------- |
| Author creates article as `draft`. Then changes status to `under review`. | Author and editor co-edit the article, then change status to `published` when ready. | Editor is now responsible for all future updates to article. Author has no update access. |

To create a structured workflow for `articles`, follow these steps.

1. First, [create a field](/configuration/data-model/fields.md#create-a-field-standard) to track the article status.
   We'll call this field `status`, but it could be named anything.
2. Define values in the `status` field _(such as `draft`, `under review` and `published`)_ for each stage in your
   content creation process.
3. Next, [create two roles](/configuration/users-roles-permissions/roles.md#create-a-role): `author` and `editor`.
4. Finally, configure
   [custom access permissions](/configuration/users-roles-permissions/permissions.md#configure-custom-permissions) for
   each role based on the value of the `status` field.
   - For the `author` role:
     - On **Create > Use Custom > Field Validation** set a filter to ensure the author can only create articles with a
       `draft` status.
     - Enable all Read Permissions.
     - On **Update > Use Custom > Item Permissions** set a filter to ensure the user can update articles with a `draft`
       or `under review` status.
     - On **Update > Use Custom > Field Validation** set a filter to ensure the user can only update article status to
       `under review`.
     - Keep Delete permissions restricted.
     - Keep Shares permissions restricted.
   - For the `editor` role:
     - Keep Create permissions restricted.
     - Enable all Read permissions.
     - On **Update > Use Custom > Item Permissions** set a filter to ensure the user can only update articles with an
       `under review` status.
     - On **Update > Use Custom > Field Validation** set a filter to ensure the user can only update status to
       `published`.
     - Keep Delete permissions restricted.
     - Keep Shares permissions restricted.

Remember, this is one simple example. You could use any collection or even multiple relationally linked collections, set
more custom access permissions, add more roles, add more stages in your workflow, _and beyond!_

:::tip

Workflows can be further enhanced with [custom interfaces](/extensions/interfaces.md) and
[flows](/configuration/flows.md).

:::
