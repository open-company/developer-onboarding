# Getting Started

Prospective users of [Carrot](https://carrot.io/) should get started by going to [Carrot.io](https://carrot.io/). The following local setup is **for developers** wanting to do development on the OpenCompany platform.

There are two main ways to approach development on the OpenCompany platform. You can focus on a single service in isolation, making changes to it and relying on tests to validate correctness, or you can get a full system running locally. Either approach is fine, it just depends on the scope of the change you are attempting as to which approach to choose.

To work on a service in isolation, follow the directions in the README of the respective repository. If you need help determining which repository contains the aspect of the platform you'd like to change, feel free to reach out to the core development team via GitHub.

To get a full development system running with all the core services, follow the steps below.

## Anatomy of a Development System

Not all services are required to be running to use the OpenCompany platform locally. A core set of "online" services are needed, and the other services that run in the background and are more queue-based only need to be run when you need them.

The core services, and the order you should focus on getting them started are:

[Storage](https://github.com/open-company/open-company-storage) - start here, requires you to get RethinkDB going.

[Authentication](https://github.com/open-company/open-company-auth) - a good next step, also uses RethinkDB.

[Interaction](https://github.com/open-company/open-company-interaction) - this rounds out the main RethinkDB data storing services. It's the first Web Socket service.

[Reminder](https://github.com/open-company/open-company-reminder) - the final RethinkDB service.

[Change](https://github.com/open-company/open-company-change) - this is the first service that requires you to get DynamoDB going.

[Notify](https://github.com/open-company/open-company-notify) - another DynamoDB service.

[Web UI](https://github.com/open-company/open-company-web) - all of these services are there to support the web client. This is the only service that uses [Boot](https://github.com/boot-clj/boot) rather than [Leiningen](https://github.com/technomancy/leiningen).

## Optional Services

[Email](https://github.com/open-company/open-company-email) - this is the most common of the "extra" services to be running. Running it results in emails actually being sent.

[Bot](https://github.com/open-company/open-company-bot) - another fairly common "extra" service to be running. Running it results in Slack messages actually being sent.

[Search](https://github.com/open-company/open-company-search) - won't cause you an issue to not have this running if you avoid the search features. If you need this service, it uses ElasticSearch.

[Slack Router / ngrok](https://github.com/open-company/open-company-slack-router) - not a common set of services to run unless you are working on Slack actions or unfurls.

[Digest](https://github.com/open-company/open-company-digest) - not a common service to run unless you're working on the digest capabilities.
