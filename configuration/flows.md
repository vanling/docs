---
description:
  Flows enable custom, event-driven data processing and task automation within Directus. Each Flow is composed of one
  Trigger, followed by a series of Operations.
readTime: 5 min read
---

# Flows

> Flows enable custom, event-driven data processing and task automation within Directus. Each Flow is composed of one
> Trigger, followed by a series of Operations.

:::tip Before You Begin

Please be sure to see the [Quickstart Guide](/getting-started/quickstart.md) to get a basic overview of the platform.

:::

:::tip Learn More

There is also dedicated API documentation on [Flows](/reference/system/flows) and
[Operations](/reference/system/operations).

:::

## What's a Flow?

<!--
Make this more narrative.
One-liner description. A flow is a trigger, followed by a tree operations.
With a Flows Object.
Control Flow: Success and Failure of operations create a tree.
Operations > options
-->

![What's a Flow?](https://cdn.directus.io/docs/v9/configuration/flows/flows/flows-20220603A/whats-a-flow-20220603A.webp)

Each Flow begins with one Trigger, which defines the action that starts the Flow. Triggers can be an event or action
within the app, an incoming webhook, a cron job, or a manual click of a button. Please see the documentation on
[Triggers](/configuration/flows/triggers) for more details.

Operations are the actions performed after the Trigger, such as accessing and managing a Collection's items, sending off
emails, pushing _in-app_ notifications, or sending webhooks, just to name a few. You can even set up divergent chains of
Flow Operations, which execute conditionally, based on whether the previous Operation passed or failed. A
[console log](/configuration/flows/operations#log-to-console) is also included to help design and troubleshoot your
Flows and ensure data is passed on as intended. Please see the documentation on
[Operations](/configuration/flows/operations) for more details.

Once a Flow begins, Directus creates a [Flow Object](#the-flow-object). This stores any data generated from the Trigger
event. When an Operation in the flow executes successfully, its data is appended onto the Flow Object. Every Operation
has access to the entire Flow Object. In other words, each Operation has access to the data from the Flow's Trigger as
well all previously executed Operations in the Flow.

## Create a Flow

<video autoplay playsinline muted loop controls title="Create a Flow">
	<source src="https://cdn.directus.io/docs/v9/configuration/flows/flows/flows-20220603A/create-a-flow-20220603A.mp4" type="video/mp4" />
</video>

To create a Flow, follow these steps:

1. Navigate to **Settings > Flows** and click <span mi btn>add</span> in the page header. A drawer will open.
2. Under **Flow Setup**, fill in a **Name** for the Flow and the following _optional_ details:
   - **Status** — Sets the Flow to active or inactive.
   - **Icon** — Adds a Material Icon used to help quickly identify the Flow.
   - **Description** — Sets a brief verbal description of the Flow.
   - **Color** — Sets a color to help identify the Flow.
   - **Activity and Logs Tracking** - Track Activity and [Logs](#logs), Activity, or neither.
3. Click <span mi btn>arrow_forward</span> to navigate to **Trigger Setup**. Select a
   [Trigger](/configuration/flows/triggers) type and configure as desired.
4. Click <span mi btn>done</span> in the Menu Header to be taken to the Flow Grid Area.\
   You will now see the Trigger Panel on the Flow Grid Area.
5. On the Trigger Panel, click <span mi>add</span> and the **Create Operation** side menu will open.
6. Choose a **Name**, an [Operation](/configuration/flows/operations) type, and configure as desired.\
   Directus will convert the unique name into an Operation Key, used on the [Flow Object](#the-flow-object).\
   If you don't choose a name, the system will auto-generate a name and key.
7. Next, click <span mi btn>done</span> in the Page Header to confirm and return to the Flow Grid Area.
8. On the newly created Operation Panel:
   - Click <span mi icon>add</span> to add an Operation to execute if the current Operation is successful.
   - Click <span mi icon>remove</span> to add an Operation to execute if the current Operation fails.
9. Repeat steps 8-10 to build out your Flow as desired.
10. **Optional:** To reconfigure a Trigger or Operation Panel, click <span mi icon>edit</span> and make any necessary
    edits.
11. **Optional:** To delete an Operation Panel, click <span mi icon>more_vert</span> then
    <span mi icon dngr>delete</span>.
12. **Optional:** To unlink an operation, click <span mi icon prmry>adjust</span> and drag.

## The Flow Object

When a Flow is triggered, Directus creates a JSON object to store all data generated within the Flow's Trigger and
operation(s).

If the trigger involved any data, such as in an Event Hook, Webhook, or the "Another Flow" triggers, the data will be
appended onto `payload` under the `$trigger` key.

As each operation executes successfully, an `operation_key` is appended to the Flow Object. If any data was generated
during the operation, it is stored under its relevant `operation_key`. Every operation has access to the Flow Object,
and therefore data stored from preceding operations.

The following JSON object is a simple example of a Flow with two Operations. The `$trigger`, `$last`, and
`$accountability` keys are included on every Flow. An Operation key will be generated for each Operation that executes
successfully.

```json
{
	"$trigger": {
		... // Data generated by the Flow trigger.
		"payload": {"some_key": "Some payload value"}
		... // More trigger data.
	},
	"$last": null, // Data from the last Operation in the flow, for easy access! Assigned NULL if no data is generated in last operation.
	"$accountability": {}, // Provides details on who/what started the flow.
	"operation_key_1": "some_value", // The data (if any) generated by the operation.
	"operation_key_2": {
		"nested_key": "Some nested value." // It will be common to have deeply nested JSON.
	},
	"operation_key_3": "{{ operation_key_2.nested_key }}" // You can use dot notation on triggers and operations to fetch values within the Flow Object.
	...
}
```

You may notice the dot-notation with double-moustache syntax on `operation_key_3`. Directus lets you use keys from the
Flow Object to use data on subsequent operations.

```json
{
	"operation_key_4": {
		"user": "{{ $accountability.user }}",
		"attribute": "operation_key_3"
	}
}
```

<!--
$trigger
$last
$accountability
-->

In the end, the Flow Object is a JSON object, and so all keys and values must follow
[JSON syntax](https://www.w3schools.com/js/js_json_syntax.asp). In general, this means the `key` must be a string, which
can contain _but cannot begin with_ a number. It can also have the following types of values.

```json
{
	"key": "string",
	"key2": 1,
	"key3": {},
	"key4": [],
	"key5": true,
	"key6": null
}
```

You cannot pass any type computational logic onto the using double-moustache syntax:

```json
{
	"key": {{ 2 + 2}}
}
```

:::tip

If you wish to perform computations within a flow, you can use the
[script operation](/configuration/flows/operations.md#script) or a
[webhook](/configuration/flows/operations.md#webhook).

:::

## Logs

<video autoplay playsinline muted loop controls title="">
	<source src="https://cdn.directus.io/docs/v9/configuration/flows/flows/flows-20220603A/logs-20220603A.mp4" type="video/mp4" />
</video>

Accessible from the Sidebar, Logs store information for each Flow execution. Each log will display information from
Triggers as well as each Operation in the Flow. To access a Flow's logs, follow these steps.

1. Navigate to **Settings > Flows** and click the desired Flow.
2. Click **<span mi icon prmry>fact_check</span> Logs** in the Sidebar. A side drawer will open, displaying the Flow's
   logs.
3. Click a log and another side drawer will open, allowing you to peer through its data.
4. When finished, click <span mi btn muted>close</span> to close the drawer.
