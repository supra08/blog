---
title: "GSOC 2020 Final Assessment - CD Foundation [Screwdriver.cd]"
date: 2020-09-29T04:05:35+05:30
slug: ""
description: "The final report for the GSOC 2020 project with Screwdriver.cd"
keywords: []
draft: true
tags: []
math: false
toc: false
---

GSOC 2020 had been a wonderful experience where I got to work on really interesting projects and tasks. Firstly, I would like to thank my mentors, [Jithin Emmanuel](github.com/jithine) and [Tiffany Kyi](https://github.com/tkyi/) for all the technical help, clearing roadblocks and important decision-making throughout the project, both before and during the programme. The same goes to all the PR reviewers from the organization for giving useful insights in the task.

For GSOC, there were **two projects** to be taken up:
* **Generate and use deploy keys**  [Issue link](https://github.com/screwdriver-cd/screwdriver/issues/1079):
    Previously, for building the projects Screwdriver either needed the repos to have public access or required the SCM Tokens to be added as a configuration. For private repositories, the SCM tokens gave user wide access that is not always desirable. In this case, **Deploy Keys** is a better option that would only give repository wide access.
    This project required the inclusion of optional generation and addition of the keys and use them for checkouts.
* **Notify SCM changes from external SCM repositories** [Issue link](https://github.com/screwdriver-cd/screwdriver/issues/1415):
    Previously Screwdriver only supported addition of webhooks to the source repositories. This project required the addition of support to add webhooks to external repositories and trigger jobs which have specifically subscribed to them.

## Work Summary
Milestones achieved:
* Generate and use deploy keys in pipeline build mechanism.
    * Implementation of 2 flows:
        * Manual generation of the keys by user and addtion to the pipeline and SCM Repo.
        * Automatic generation and addtion of public key to the repo with **Github-API** as well as addition of private key as a secret. Second level validation to activate this is setting a flag in the API config and UI.
    * Unit Tests and Feature tests for all the features.
* Notification from external SCM repositories:
    * Refactor to add webhooks to **multiple repos** with custom events.
    * Add logic to fetch **triggered pipelines** from the datastore during subscribed hook events.
    * Add logic to **select and trigger specific subscribed jobs** during subscribed hook events.
### Pull Requests
* Generation and use of Deploy Keys [Issue link](https://github.com/screwdriver-cd/screwdriver/issues/1079):
    * Implement handling and checkout with deploy keys: [https://github.com/screwdriver-cd/scm-github/pull/159](https://github.com/screwdriver-cd/scm-github/pull/159)
    * Auto keypair generation and auto pushing public key to SCM: [https://github.com/screwdriver-cd/scm-github/pull/163](https://github.com/screwdriver-cd/scm-github/pull/163)
    * Add addDeployKey method to scm-router: [https://github.com/screwdriver-cd/scm-router/pull/23](https://github.com/screwdriver-cd/scm-router/pull/23)
    * Add addDeployKey method to scm-base: [https://github.com/screwdriver-cd/scm-base/pull/79](https://github.com/screwdriver-cd/scm-base/pull/79)
    * Add relevant fields in the schemas: [https://github.com/screwdriver-cd/data-schema/pull/404](https://github.com/screwdriver-cd/data-schema/pull/404)
    * Add autoDeployKeysGenerationEnabled in scm-router: [https://github.com/screwdriver-cd/scm-router/pull/24](https://github.com/screwdriver-cd/scm-router/pull/24)
    * Validation check in the UI: [https://github.com/screwdriver-cd/ui/pull/568](https://github.com/screwdriver-cd/ui/pull/568)
    * Final integration in the API: [https://github.com/screwdriver-cd/screwdriver/pull/2157](https://github.com/screwdriver-cd/screwdriver/pull/2157)
    * Refactors in the scm-github module: [https://github.com/screwdriver-cd/scm-github/pull/168](https://github.com/screwdriver-cd/scm-github/pull/168)
    * Refactor in the scm-base to return non-promise value: [https://github.com/screwdriver-cd/scm-base/pull/81](https://github.com/screwdriver-cd/scm-base/pull/81)
    * Refactor in the scm-router to return non-promise value: [https://github.com/screwdriver-cd/scm-router/pull/25](https://github.com/screwdriver-cd/scm-router/pull/25)
    * Fix request scope format in scm-github: [https://github.com/screwdriver-cd/scm-github/pull/169](https://github.com/screwdriver-cd/scm-github/pull/169)
    * Implement auto-secret-addition in the model: [https://github.com/screwdriver-cd/models/pull/471](https://github.com/screwdriver-cd/models/pull/471)
    * Enable to auto-upgradation to ssh-based checkout: [https://github.com/screwdriver-cd/scm-github/pull/171](https://github.com/screwdriver-cd/scm-github/pull/171)
    * Set allow-in-PR true for deploy key secret: [https://github.com/screwdriver-cd/screwdriver/pull/2176](https://github.com/screwdriver-cd/screwdriver/pull/2176)
    * Allow autoKeysGeneration flag in the update endpoint schema: [https://github.com/screwdriver-cd/data-schema/pull/407](https://github.com/screwdriver-cd/data-schema/pull/407)
    * [WIP] Implementation of data-driven feature test to test the deploy key feature: [https://github.com/screwdriver-cd/screwdriver/pull/2158](https://github.com/screwdriver-cd/screwdriver/pull/2158)
    * Update the docs with the deploy keys feature: [https://github.com/screwdriver-cd/guide/pull/401](https://github.com/screwdriver-cd/guide/pull/401)
The feature is officially out and published here: [https://blog.screwdriver.cd/post/626369335636656128/latest-product-updates](https://blog.screwdriver.cd/post/626369335636656128/latest-product-updates)

* Notification from external SCM repositories [https://github.com/screwdriver-cd/screwdriver/issues/1415](https://github.com/screwdriver-cd/screwdriver/issues/1415):
    * Add fields for the updated screwdriver and model config in the schema: [https://github.com/screwdriver-cd/data-schema/pull/408](https://github.com/screwdriver-cd/data-schema/pull/408)
    * Add subscribed field in the parsed config: [https://github.com/screwdriver-cd/config-parser/pull/109](https://github.com/screwdriver-cd/config-parser/pull/109)
    * Add feature to handle custom events in scm-github: [https://github.com/screwdriver-cd/scm-github/pull/174](https://github.com/screwdriver-cd/scm-github/pull/174)
    * Add handlers for subscribed repos, modify addWebhook for multiple URLs and alternate paths for subscribed hook events: [https://github.com/screwdriver-cd/models/pull/472](https://github.com/screwdriver-cd/models/pull/472)
    * Final API integration and logic to handle external hook notifications: [https://github.com/screwdriver-cd/screwdriver/pull/2187](https://github.com/screwdriver-cd/screwdriver/pull/2187)

### Pre GSOC work:
* Allow editing of existing PR comments by bot: [https://github.com/screwdriver-cd/scm-github/pull/152](https://github.com/screwdriver-cd/scm-github/pull/152)
* Config validation for slack settings: [https://github.com/screwdriver-cd/notifications-email/pull/21](https://github.com/screwdriver-cd/notifications-email/pull/21), [https://github.com/screwdriver-cd/notifications-slack/pull/25](https://github.com/screwdriver-cd/notifications-slack/pull/25)

## Conclusion
I feel I've have completed almost all of the goals mentioned in the PR. The **Generation and use of Deploy Keys** PRs are all merged and the feature has been published. For the **Notification from external SCM repositories**, all of the PRs have been made and some have been approved. The feature tests are the ones that are left in the implementation and are in WIP. But those being similar to lot of existing tests can be easily added. 

## Future Work
A couple of enhancements were discussed to be added to both the products and were decided to be taken up as secondary goals. Some of them were:
* Implement **rotation** of deploy keys that would require enabling of storage of the keys in the data store in a secure way.
* Add **label based selector** of jobs that would subscribe to external pipelines.
