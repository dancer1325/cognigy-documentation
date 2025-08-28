---
title: "Snapshots"
slug: "snapshots"
description: "Snapshots are immutable versions of an AI Agent in Cognigy.AI that includes various resources such as Flows, Lexicons, Connections, Functions, and more. Snapshots can be used to create a copy of an AI Agent for deployment, to create backups, or to revert to an earlier state if needed."
hidden: false
tags:
  - snapshots
  - deployment
  - version control
  - backup
  - production
  - development
  - staging
---

# Snapshots

[![Version badge](https://img.shields.io/badge/Updated in-v4.47-blue.svg)](../../release-notes/4.47.md)

* _Snapshot_
  * := AI Agent's IMMUTABLE version /
    * data is encrypted
      * Reason: 🧠it can include sensitive information (_Example:_ [Connections](../build/connections.md)) 
    * 's resources
      - [AI Agents](../empower/agentic-ai/overview.md)
      - [Connections](../build/connections.md)
      - [Extensions](../build/extensions.md)
      - [Flows](../build/flows/overview.md)
      - [Functions](../build/functions.md)
      - [Large Language Models](../empower/llms/overview.md)
      - [Lexicons](../empower/nlu/slots/user-defined/lexicon.md)
      - [Localization](../build/translation-and-localization/localization.md)
      - [Natural Language Understanding (NLU) Connectors](../empower/nlu/external/nlu-connectors/overview.md)
      - [Playbooks](../test/playbooks.md)
      - [Settings](../build/projects.md#working-with-projects)
      - [Tokens](../build/tokens.md)
      - [Trained NLU Intent models](../empower/nlu/intents/overview.md)
  * ways to use
    * 👀publish the snapshots -- , through [Endpoints](endpoints/overview.md), to -- users 👀
      * see [here](#deploy-a-snapshot)
    * download a Snapshot (-- as -- ".csnap") & use | Cognigy.AI
      * steps
        1. TODO: In the left-side menu of the Project, go to **Deploy > Snapshots**.
        2. On the **Snapshots** page, click **Upload Snapshot**.
        3. Select a Snapshot with the `.csnap` format from your computer. When the file starts uploading, you will see a dialog window with a progress bar.
        4. Once the file has been uploaded, a new [task](../build/projects.md), titled **Upload Snapshot**, will be created. To view the task, click ![task-menu](../../_assets/icons/task-menu.svg) in the upper-right corner.
  * OTHER supported actions
    * [Back up your AI Agent](#restore-a-snapshot)
    * [Transfer an AI Agent BETWEEN DIFFERENT CI/CD environments](#deploy-ai-agents-to-production-with-snapshots)

## Restrictions

- `text-embedding-3-model` [model](../empower/llms/model-support-by-feature.md)
  - increases the Snapshots' size
    - -> ⚠️longer download & upload times⚠️
- Cognigy.AI's earlier versions
  - ❌might NOT support Snapshots / created | NEWER versions❌

## Limitations

- TODO: You can create up to 10 Snapshots per AI Agent. For dedicated SaaS and on-premises installations, you can change this value using the `MAX_AMOUNT_SNAPSHOTS_IN_AGENT` environment variable in `values.yaml`.
- You can store up to 256 MB of Snapshots. If you reach this limit, you must first delete other Snapshots before creating or uploading new ones. For dedicated SaaS and on-premises installations, you can increase the maximum memory for Snapshots using the `SNAPSHOT_MAX_FILE_SIZE` environment variable in `values.yaml`.
- You have up to 1 day to download a Snapshot after you've prepared the download. After this period, you need to prepare the download again.

## Working with Snapshots

!!! danger
    If you delete a Snapshot that you use with an Endpoint in production, the AI Agent connected to that Endpoint stops working.

=== "GUI"
    You can create, delete, and [restore](#restore-a-snapshot) Snapshots as well as download and upload them in **Deploy > Snapshots**. To download a Snapshot, first select ![vertical-ellipsis](../../_assets/icons/vertical-ellipsis.svg) **> Prepare Download**, then click ![snapshot-download](../../_assets/icons/snapshot-download.svg) when the download is ready.

=== "API"
    You can view, create, delete, and [restore](#restore-a-snapshot) Snapshots using the [Cognigy API](https://api-dev.cognigy.ai/openapi#get-/v2.0/snapshots) as well as download and upload them.

=== "CLI"
    You can create Snapshots using the [Cognigy CLI](https://github.com/Cognigy/Cognigy-CLI).

### Deploy a Snapshot

To deploy a Snapshot, select it from the **Snapshot** list in the [Endpoint settings](endpoints/overview.md).

While an Endpoint is active, you can replace a Snapshot or remove it in the Endpoint settings. Active sessions continue to use the old Snapshot until they time out or you delete the old Snapshot. If you replace a Snapshot and delete it, the session is reset using the new Snapshot.

If you use the [Persistent History](https://github.com/Cognigy/Webchat/blob/master/docs/persistent-history.md) for Webchat, the user's browser retains the chat history. Even after the session expires, users still see old messages, but any new message starts a new session.

### Restore a Snapshot

You can use Snapshots to back up AI Agents versions. With this approach, you can restore a back up version if the new AI Agent doesn't perform as intended.

Before restoring a Snapshot, be aware of the following:

- **Restoring a Snapshot overwrites all the current AI Agent's resources**. All the AI Agent's resources are deleted before they're recreated from the Snapshot you restore. In case you have active Endpoints pointing to the Flows of the AI Agent, the conversations stop working because the Flows are deleted.
- **Restoring a Snapshot might require manual changes to the Endpoints**. Restoring a Snapshot automatically updates only the Endpoints that are assigned to the primary Locale. You need to update Endpoints assigned to non-primary Locales manually. These Endpoints are marked with a red dot next to the Endpoint name after you restore the Snapshot, meaning that the settings are not complete.

## Deploy AI Agents to Production with Snapshots

The following three scenarios show the recommended methods for deploying AI Agents into production.

??? info "Develop an AI Agent with a Single Project"

    With Snapshots, you can make your AI Agents available through Endpoints while you work on the next version of your AI Agent:

    1. Develop and test Flows, Lexicons, Intents, and all other resources as usual. Eventually, you reach an AI Agent version that you want to publish to the users.
    2. Create a Snapshot of the AI Agent. You don't have to prepare a downloadable Package for the Snapshot because everything remains in the current Project.
    3. To release the AI Agent version of the Snapshot, go to the Endpoint settings, for example, in the Webchat Endpoint settings, and select the new Snapshot and the Flow.<br>Your production environment now uses the AI Agent from the Snapshot.

    Since you are using resources from the Snapshot in production, developers can change the Flows in the AI Agent without breaking the current conversations with your customers.

??? info "Develop an AI Agent with Multiple Projects"

    You want to build an AI Agent for FAQs on your website. In this case, you have two Projects:

    - **FAQ Bot development** — a Project for developers to build and test the AI Agent.
    - **FAQ Bot production** — a Project for the production environment.

    1. Developers only have access to the FAQ Bot development Project, in which they can develop and test AI Agents. Eventually, they reach an AI Agent version that you want to publish to the users.
    2. Create a Snapshot of the AI Agent in the FAQ Bot development Project and download it.
    3. To release the AI Agent version of the Snapshot, upload the Snapshot to the FAQ Bot production Project. Then, select the uploaded Snapshot in the Endpoints settings of your production Endpoint.

??? info "Develop an AI Agent with a CI/CD Environments"

    You have the following environments:

    - **Dev** — an environment for development purposes.
    - **Staging** — an environment for staging purposes, where acceptance tests are conducted for newer versions.
    - **Production** — an environment for publishing AI Agents to users.

    1. Developers only have access to Projects in the Dev installation, in which they build AI Agents, design new experiences, or improve already existing solutions. Eventually, they reach an AI Agent version that you want to publish to the users.
    2. Create Snapshots of the AI Agents in the Dev installation, download the Snapshots, and provide them to the teams working on the Staging installation.
    3. People working on the Staging installation receive the Snapshots and upload them to their Projects. They select the uploaded Snapshots in their Endpoint settings and run acceptance tests.
    4. If they find errors, they inform their development teams, who fix the problems and provide new Snapshots. If tests are successful, the Snapshots are moved to the Production installation.
    5. To release the AI Agent version of the Snapshots, upload them to the Production installation and select the uploaded Snapshots in the production Endpoints.

## More Information

- [Endpoints](endpoints/overview.md)
- [Flows](../build/flows/overview.md)
