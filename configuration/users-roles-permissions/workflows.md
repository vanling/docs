# Workflows

> Workflows are a way to add structured stages to the flow of content authoring. This enables one to do things like pass
> off work in progress between multiple roles, create multi-signature approvals, build decision trees, _and beyond!_

A workflow is defined with custom access permissions. Basically, you configure each role's custom access permissions on
some collection(s), defining what they can and can't have access to, so that the each role's CRUDS permissions change at
each stage in the workflow.

:::tip

At times, custom access permissions will not be enough to get the desired result. However, workflows can be further
enhanced with [custom interfaces](/extensions/interfaces.md) and [flows](/configuration/flows.md).

:::

## How it Works

<video title="Configure Workflows" autoplay playsinline muted loop controls>
	<source src="https://cdn.directus.io/" type="video/mp4" />
</video>

There are an infinite number of possible workflows you could configure, so it is impossible to exhaustively describe
anything and everything you could do. However, to give an example, we will configure one workflow on an `articles`
collection. In our workflow, someone from the `author` role will create an article, then pass it to an `editor` role who
will co-edit and publish the article.

To create a structured workflow for `articles`, follow these steps.

1. First, [Create a Field](/configuration/data-model/fields.md#create-a-field-standard) to track the article status —
   we'll call this field `status`, but it can be named anything.
2. Next, create the `author` and `editor` roles.
3. Finally, configure each role's permissions based on the possible values of that `status` field.
   - The `author` is granted create and update permissions when the article status and can assign `draft` and
     `ready for review`.
   - Once assigned `ready for review`, the `editor` is granted permissions to update the article and to update status to
     `published`.

Remember, this is one example among endless possibilities. You could add more roles and stages to the flow, create more
complex custom access permissions, configure workflows across multiple relationally linked collections, _and beyond!_
