---
description:
  A Trigger initiates a Flow on some internal or external event, such as an in-app activity, incoming webhook, cron job,
  execution of Operation(s) from other Flows, and beyond!
readTime: 3 min read
---

# Triggers

> A trigger defines the event that starts your flow. This could be from an internal or external activity, such as
> changes to data, logins, errors, incoming webhooks, cron jobs, operations from other flows, or even the click of a
> button within the Data Studio.

:::tip Before You Begin

Please be sure to read the documentation on [Flows](/configuration/flows).

:::

## Event Hook

![Event Hooks](https://cdn.directus.io/docs/v9/configuration/flows/triggers/triggers-20220603A/event-hook-20220602A.webp)

Event Hooks are triggered by events within the platform. The logic is based on [Custom API Hooks](/extensions/hooks).
Any data generated will be stored in the `$trigger`'s `payload` on the Flow Object.

- **Type** — Choose the type of Event Hook:
  - [Filter (Blocking)](#filters) — Filters fire "blocking" right before the database transaction is committed, allowing
    you to validate or transform the payload or even prevent completion outright.
  - [Action (Non-Blocking)](#actions) — Actions do not block anything. A non-blocking action is mostly useful for
    completing tasks in response to an event, without slowing the API.
- **Scope** — Set the events that trip this trigger.
- **Collections** — Set the collections _whose events_ trip this trigger.
- **Response Body** — This is optional and only for Filter (Blocking) events. Defines Flow Object data to return to the
  event, overwriting the original payload. Choose to return:
  - **Data of last Operation** — Same as `$last`.
  - **All Data** — Same as `$all`
  - **Other** — Input any `key` from the Flow Object.

:::tip Response Body

If no **Response Body** is provided, the original payload will be transacted to the database.

:::

::: tip Events with Multiple Items

If you create, update or delete multiple items in a single request, the Flow will trigger once for each and every item.
If you create three items, the flow runs three separate times. On each run, just one item will be in the payload.

:::

### Filters

![Blocking Events](image.webp)

A Filter is a blocking event trigger. This means it will pause the event, let the whole flow run, then commit the
transaction to the database. During this process, you have the option to pass in the event's payload and validate or
transform it before committing it to the database, or even prevent completion outright.

For example, let's say you configure the scope to be `item.create` on some collection(s). When you send a request to
create items on the relevant collection(s):

- A request to create an item is sent to Directus.
- The create item event pauses.
- The flow's trigger is tripped.
- The event's payload passes into the `$trigger` of the flow.
- The flow runs.
- **Optional:** If you defined a **Response Body**, this replaces the event's payload.
- The transaction is committed to the database or cancelled, based on your Flow logic.

:::tip Cancelling Transactions

To completely cancel a transaction, you'll need to throw an error within a
[script operation](/configuration/flows/operations.md#script).

:::

:::tip Debugging Filters

If you use a tool like postman when you go to debug your Filter event, the **Response Body** configured on the trigger
is returned to the event hook. The event hook sends another response to postman. Therefore, you won't see your flow's
**Response Body** data within postman.

:::

### Actions

![Non-Blocking Events](image.webp)

An Action is a non-blocking event trigger. This lets the event commit its transaction to the database without waiting
for the flow to finish. Therefore, Actions give you a more performant API, but you have no ability to validate or
transform the payload before the transaction is committed to the database.

## Webhook

![Webhook](https://cdn.directus.io/docs/v9/configuration/flows/triggers/triggers-20220603A/webhook-20220602A.webp)

Triggers on an incoming HTTP request to: `/flows/trigger/:this-webhook-trigger-id` which you can copy from the webhook
trigger panel. You also have the option to return Flow Object data via **Response Body**.

- **Method** — Choose whether the flow will be triggered by a `GET` or `POST` request from the dropdown.
- **Asynchronous** — Toggle whether or not the trigger responds asynchronously.
- **Response Body** — Returns Flow Object data as a response body to the incoming HTTP request. Choose to return:
  - **Data of last Operation** — Same as `$last`.
  - **All Data** — Same as `$all`.
  - **Other** — Input any `key` from the Flow Object.

:::tip

If no data is defined in **Response Body**, the flow responds with an empty array.

:::

## Schedule

![Schedule a Cron Job](https://cdn.directus.io/docs/v9/configuration/flows/triggers/triggers-20220603A/cron-20220602A.webp)

This Trigger enables you to create data at scheduled intervals, via 6-point cron job syntax.

- **Interval** — Set the cron job interval to schedule when the Flow triggers.

## Another Flow

![Another Flow](https://cdn.directus.io/docs/v9/configuration/flows/triggers/triggers-20220603A/another-flow-20220602A.webp)

This trigger executes a flow via the [Trigger Flow](/configuration/flows/operations#another-flow) operation, allowing
you to chain flows.

- **Response Body** — Returns Flow Object data back to the operation that triggered the flow. Choose to return:
  - **Data of last Operation** — Same as `$last`.
  - **All Data** — Same as `$all`.
  - **Other** — Input any `key` from the Flow Object.

## Manual

![Manual](https://cdn.directus.io/docs/v9/configuration/flows/triggers/triggers-20220603A/manual-20220602A.webp)

This Trigger starts your Flow on a manual click of a button within the Directus App. When you use this trigger, a
**Flows** menu containing a button will appear in the sidebar of the specified collection page(s) and/or its item pages.

The manual trigger must take in item ID(s) to activate. From the item page, the current item's ID is passed in. From the
collection page, the button will be _grayed out_ until you select some number of items. These item IDs are passed into
the the flow's `$trigger` object.

- **Collections** — Choose the Collection(s) to add the button to.
- **Location** — Choose to display the button within the Item Page, Collection Page, or both.
- **Asynchronous** — Toggle whether or not the Flow executes asynchronously.
