# Developer Onboarding

> Transparency is the currency of trust, the open sharing of information among parties involved. Often, a decline in transparency is the first indicator of trouble. As transparency declines, trust declines.

> -- [Christopher S. Penn](http://www.christopherspenn.com/)

Companies struggle to keep everyone on the same page. People are hyper-connected in the moment but still don't know what's happening across the company. Employees and investors, co-founders and execs, customers and community, they all want more transparency. The solution is surprisingly simple and effective - great company updates that build transparency and alignment.

With that in mind we designed the [Carrot](https://carrot.io/) software-as-a-service application, powered by the open source [OpenCompany platform](https://github.com/open-company). The product design is based on three principles:

1. It has to be easy or no one will play.
2. The "big picture" should always be visible.
3. Alignment is valuable beyond the team, too.

Carrot simplifies how key business information is shared with stakeholders to create alignment. When information about growth, finances, ownership and challenges is shared transparently, it inspires trust, new ideas and new levels of stakeholder engagement. Carrot makes it easy for founders to engage with employees and investors, creating alignment for everyone.

[Carrot](https://carrot.io/) is GitHub for the rest of your company.

Transparency expectations are changing. Organizations need to change as well if they are going to attract and retain savvy employees and investors. Just as open source changed the way we build software, transparency changes how we build successful companies with information that is open, interactive, and always accessible. Carrot turns transparency into a competitive advantage.

To get started, head to: [Carrot](https://carrot.io/)

Information for new OpenCompany platform developers:

* [Technical Values](#technical-values)
* [Technical Priorities](#technical-priorities)
* [Process Priorities](#process-priorities)
* [Architectural Priorities](#architectural-priorities)
* [System Architecture](#system-architecture)
* [Coding Guidelines](#coding-guidelines)
* [Tools](#tools)
* [Code of Conduct](#code-of-conduct)
* [License](#license)


## Technical Values

**KISS** - **K**eep **I**t **S**imple **S**tupid - Complexity is our enemy. We need to solve the complexity inherent in the problems we face using simple solutions, and avoid introducing complexity that isn't inherent. Solution complexity is rejected, rather than dealt with. We ask [5 whys](http://en.wikipedia.org/wiki/5_Whys) about complexity. What can be relaxed or changed to make the complexity go away?

**YAGNI** - **Y**ou **A**in't **G**onna **N**eed **I**t - This is the answer to much introduced complexity. We force complexity upon ourselves by being so good at asking "But what if...?" Instead, we build for the problems we have today, not for the problems we think we might have someday. None of us are fortune tellers. We're mostly wrong.

**No Punting** - No one owns an app, a system a section of code, or a problem. We don’t punt issues off to others just because we weren't involved in the inception. Everyone works on, and fixes everything. Not knowing how something works doesn't mean you don’t work on it or fix it, it means the opposite! We seek out every opportunity to work on things we don't know about (and we fill in any missing documentation as we learn). Everyone on the team is full-stack. No specialists.


## Technical Priorities

1. Simple & DRY with a single responsibility
  * **Simple** - see KISS and YAGNI above.
  * **DRY** - **D**on't **R**epeat **Y**ourself - there's no place for copy and paste. It inevitably bites you in the ass.
  * **Single Responsibility** - units of software do 1 thing well, and only 1 thing. This is fractal and applies to lines of code, functions, classes, services, systems, and apps.
2. Transparent & manageable - logs, analytics, error reporting, separation of config from code, and real-time adjustments are good

By following these 2 top technical priorities, we get two important emergent properties: **robustness** and **scalability**

Our full set of technical priorities, in order:

1. Simple & DRY with a single responsibility
2. Transparent & manageable
3. Secure
4. Consistent
5. Documented
6. Tested

Developers are held accountable to these technical priorities. Code reviews are structured around these technical priorities.

Living these priorities requires hard thinking about the technical problems we face. Code that works is not the goal. Code that works by embodying these priorities, and the appropriate trade-offs between them, is the goal.


## Process Priorities

Production code gets tested. Tests run in [continuous integration (CI)](https://en.wikipedia.org/wiki/Continuous_integration). Not all code needs all kinds of tests (unit, integration, system, acceptance, ...). Pragmatism still rules and strict [test-driven development (TDD)](https://en.wikipedia.org/wiki/Test-driven_development) is not important to us (we prefer our own acronym of TDD, thinking-driven development), but automated test development is a core skill of all our developers and test automation is a key consideration in code reviews.

All services do their best to trap and report errors. Error handling and alerting is a key consideration in code reviews.

Deployment is automated. It's not done until it ships with automated deployment. It ships as soon as possible. Risk is mitigated by shipping small changes  incrementally.

All developers write documentation. Documentation is part of code review: did the appropriate docs get created and updated for the addition/change? 

Documentation is well-organized. Even when good docs exist, if the person that needs them doesn't know where they are or that they exist, they do no good. 

Code is documented. Comments are good! It’s always critical to comment on why this code is doing what it's doing. It’s also important to comment on what the code is doing, when that's not completely obvious.


## Architectural Priorities

We build services, not monoliths. Services compose in architecturally sound ways to deliver services to layers higher up the stack.

Libraries are better than micro-frameworks, which are better than monolithic frameworks.

Observers are better than Pub/Sub, which is better than Queued Data Pipelines, which are better than Synchronous Services. Observers and queues are more robust than synchronous services (e.g. HTTP) and should be preferred where appropriate.

We use the right language for the job. We are a polyglot team. We prefer polyglot developers who have learned how to efficiently learn new things.

The right language for the job is generally obvious due to a language feature or library implemented in the language that makes the job a much easier match for our [technical priorities](#technical-priorities). The job becomes much simpler, or more robust, or easier to test, etc.

If the job is general purpose utility computing with no language feature or library advantages, we use [Clojure and ClojureScript](#tools) as our default.


## System Architecture

OpenCompany uses a distributed microservices architecture. Many services provide access to themselves via HTTPS and/or
WSS APIs. Others provide their services through the SQS queuing service. A key linchpin is JWToken authentication
tokens which are used to provide cross-service authentication and user state. A typical use of the service is through
the OpenCompany web client.

![System Diagram](https://cdn.rawgit.com/open-company/developer-onboarding/master/OC-System.svg)

### Quick Glossary

[Auth RethinkDB](https://github.com/open-company/open-company-auth#technical-design) - RethinkDB data store of users

[Auth Service](https://github.com/open-company/open-company-auth) - Authentication microservice, handles user authentication, Slack single sign-on, and JWToken generation (HTTPS)

[AWS CloudFront](https://aws.amazon.com/cloudfront/) - Content delivery network

[AWS S3](https://aws.amazon.com/s3/) - Simple Storage Service

[AWS SES](https://aws.amazon.com/ses/) - Simple Email Service

AWS SNS/SQS - [Simple Notification Service](https://aws.amazon.com/sns/) and [Simple Queuing Service](https://aws.amazon.com/sqs/)

[Bot Service](https://github.com/open-company/open-company-bot) - Slack bot microservice, handles Slack integration other than single sign-on (HTTPS/SQS)

[Change DynamoDB](https://github.com/open-company/open-company-change#technical-design) - DynamoDB store of change and see events

[Change Service](https://github.com/open-company/open-company-change) - Change tracking microservice, handles read/unread state per user and async notifications of new content (WSS)

[Notify Service](https://github.com/open-company/open-company-notify) - Service that handles general purpose notifications to users and notifications for mentions in comments and posts.

[Reminder Service](https://github.com/open-company/open-company-reminder) - Service that supports scheduling reminders to users.

[Email Service](https://github.com/open-company/open-company-email) - Microservice for outbound content and transactional emails (SQS)

[FileStack](https://www.filestack.com/) - File upload as a service

[Google Sheets](https://www.google.com/sheets/about/) - Spreadsheets in the Google cloud

[Interaction Service](https://github.com/open-company/open-company-interaction) - Engagement microservice, handles user engagement with reactions and comments (HTTPS, WSS)

[Proxy Service](https://github.com/open-company/open-company-proxy) - Charting microservice, handles extracting charts from Google Sheets (HTTPS)

[Search Service](https://github.com/open-company/open-company-search) - Microservice for searching and indexing carrot data. (SQS and Elastic Search)

[Slack Router](https://github.com/open-company/open-company-slack-router) - Microservice for receiving slack API events and publishing them to SNS.

[Slack](https://api.slack.com/) - Company chat application

[Storage Service](https://github.com/open-company/open-company-storage) - Persistence microservice, handles information data management and access control (HTTPS)

[Storage RethinkDB](https://github.com/open-company/open-company-storage#technical-design) - RethinkDB data store of user content

[Web UI](https://github.com/open-company/open-company-web) - ClojureScript/React mobile and desktop web client


## Coding Guidelines

We're much more interested in ensuring code follows the values and priorities described above than that it follows any particular coding standard. Code formatting is just the small tip of a very large iceberg; poorly formatted code can be programmatically tidied, but poorly done thinking can't be programmatically rethought. That being said, here are some code guidelines we look for:

* Do your best to conform to the coding style that's already there in the repository... That means we like it.
* Use 2 soft spaces for indentation for white space irrelevant code/data. Use 4 soft spaces for white space relevant code/data.
* Don't leave trailing spaces after lines.
* Don't leave trailing new lines at the end of files.
* We prefer `defn-` to `:^private` where its use is possible
* Write comments. Comments are good.
* Write tests. Tests are good.
* Don't submit über pull requests, keep your changes focused and atomic.
* Submit pull requests to the `mainline` branch of repos.
* Have fun!

### Pull Request Process

The purpose of the pull request process is to ensure code and product quality. The reviewer has 2 important responsibilities: code quality via code review, and product quality via QA of the change, ensuring both that the new functionality or fix works as expected and that other related parts of the product haven't regressed.

1. The creator of the pull request is responsible for adequately describing the change being made. The main components of the description are what changed, how it changed, and why it changed, and what steps were taken to fully QA the change.
1. The creator of the pull request marks them-self as the assignee, which signifies they are the primary submitter of the change and will be the one to respond to reviewer's question, comments and requests for changes. If at some point another developer becomes the primary contributor of the change, the assignee is changed.
1. At this point though, the PR is not yet ready for review, it may still be in progress. Once it is ready to be reviewed, the assignee adds the label "ready for review". In the unusual case that the assignee wants the PR reviewed by a specific developer, they can also assign that developer with the reviewer field. NB: This is not usually done and you need a good reason to want a specific reviewer.
1. At this point, any other developer that sees the PR should do the review, assigning themself with the reviewer field, removing the "ready for review" label, and labeling it with "reviewing". The reviewer reviews the code and performs QA, repeating the steps provided in the PR description *and* thinking critically about what additional QA steps may have been missed by the assignee.
1. Now the usual back and forth commences between the assignee and the reviewer consisting of questions, comments, ideas, requests, suggestions and changes. NB: Reviews are about the code, not about the people, as an assignee, try to remain objective and not take anything from the reviewer personally, and as a reviewer, be kind and stay focused on the code. 
1. Once the code is in a place that both the assignee and reviewer are happy with it, it is the assignees responsibility to merge the PR and get the code deployed into production. Any special notes on deployment steps or dependencies should be included in the PR for the reviewer.

## Tools

If the job is general purpose utility computing with no language feature or library advantages, we use Clojure and ClojureScript as our default.

### Clojure

[Clojure](http://clojure.org/) is a Lisp that targets the [Java Virtual Machine (JVM)](https://en.wikipedia.org/wiki/Java_virtual_machine).

Tutorials:

* [Clojure From the Ground Up](https://aphyr.com/posts/301-clojure-from-the-ground-up-first-principles)
* [Clojure for the Brave and True](http://www.braveclojure.com/)

References:

* [Clojure Distilled](https://yogthos.github.io/ClojureDistilled.html)
* [Cheatsheet](http://jafingerhut.github.io/cheatsheet/grimoire/cheatsheet-tiptip-cdocs-summary.html)
* [ClojureDocs](http://clojuredocs.org/) - a community-powered documentation and examples repository for Clojure
* [Quickref for Clojure Core](http://clojuredocs.org/quickref)
* [Clojure Quick Reference](http://faustus.webatu.com/clj-quick-ref.html)
* [Clojure Codex](http://www.stuttaford.me/codex/) - curated links

Libraries:

* [A Directory of Clojure Libraries](http://clojure-doc.org/articles/ecosystem/libraries_directory.html)
* [Clojure Libraries](https://clojure-libraries.appspot.com/)
* [Luminus Useful Libraries for Clojure Web Development](http://www.luminusweb.net/docs/useful_libraries.md)
* [The Clojure Toolbox](http://www.clojure-toolbox.com/)

Other:

* [Leiningen](http://leiningen.org/) - declarative dependency management and build tool (via plugins)
* [Boot](https://github.com/boot-clj/boot) - functional build and dependency management tool
* [Clojurians on Slack](http://clojurians.net/)
* [Clojure email list](https://groups.google.com/d/forum/clojure)

### ClojureScript

[ClojureScript](https://github.com/clojure/clojurescript) is a Clojure compiler that targets JavaScript to run Clojure in the browser (or on server-side JavaScript interpreters).

Tutorials:

* [ClojureScript Tutorial](http://www.niwi.be/cljs-workshop/)
* [ClojureScript Unraveled](http://funcool.github.io/clojurescript-unraveled/)
* [ClojureScript Koans](http://clojurescriptkoans.com/)

Reference:

* [ClojureScript Cheatsheet](http://cljs.info/cheatsheet/)
* [ClojureScript Syntax in 15 minutes](https://github.com/shaunlebron/ClojureScript-Syntax-in-15-minutes)
* [ClojureScript translations from JavaScript](http://kanaka.github.io/clojurescript/web/synonym.html)

Other:

* [CLJSFiddle](http://cljsfiddle.net/) - try your ClojureScript online
* [ClojureScript email list](https://groups.google.com/d/forum/clojurescript)
* [core.async](https://github.com/clojure/core.async) - library for async communication, often used in ClojureScript ([reference](https://clojure.github.io/core.async/))

### Rum

[Rum](https://github.com/tonsky/rum) is a ClojureScript interface to [React](http://facebook.github.io/react/).

References:

* [Rum wiki](https://github.com/tonsky/rum/wiki)

Libraries:

* [Derivatives](https://github.com/martinklepsch/derivatives) - library for deriving needed component state from state atom

Other:

* [Rum workshop video](https://www.youtube.com/watch?v=RqHnxkU9TZE)

### RethinkDB

[RethinkDB](http://rethinkdb.com/) is an open-source, multi-modal (document, key/value, relational) database.

Tutorials:

* [Quick Start](http://rethinkdb.com/docs/quickstart/)
* [Ten-minute Guide](http://rethinkdb.com/docs/guide/javascript/)

Reference:

* [Clojure API Reference](http://apa512.github.io/clj-rethinkdb/)

Other:

* [clj-rethinkdb](https://github.com/apa512/clj-rethinkdb) - a RethinkDB client for Clojure


### Elasticsearch

Elasticsearch is an open-source, distributed, JSON-based search and analytics engine designed for horizontal scalability, maximum reliability, and easy management.

References:

* [Elastic Stack and Product Documentation](https://www.elastic.co/guide/)

Libraries:

* [http://clojureelasticsearch.info/](Elastich) - an Elasticsearch client for Clojure


## Code of Conduct

Please note that this project is released with a [Contributor Code of Conduct](https://github.com/open-company/developer-onboarding/blob/master/CODE-OF-CONDUCT.md). By participating in this project you agree to abide by its terms.


## License

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" href="http://purl.org/dc/dcmitype/Text" property="dct:title" rel="dct:type">Developer Onboarding</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="http://carrot.io/" property="cc:attributionName" rel="cc:attributionURL">OpenCompany, LLC.</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

Copyright © 2015-2018 OpenCompany, Inc.
