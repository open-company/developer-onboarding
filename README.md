# Developer Onboarding

Information for new developers.

* [Technical Values](#technical-values)
* [Technical Priorities](#technical-priorities)
* [Architectural Priorities](#architectural-priorities)
* [Process Priorities](#process-priorities)
* [Tools](#tools)
* [Code of Conduct](#code-of-conduct)
* [License](#license)

## Technical Values

Technical Values

**KISS** - *K*eep *I*t *S*imple *S*tupid - Complexity is our enemy. We need to solve the complexity inherent in the problems we face using simple solutions, and avoid introducing complexity that isn’t inherent. Solution complexity is rejected, rather than dealt with. We ask [5 whys](http://en.wikipedia.org/wiki/5_Whys) about complexity. What can be relaxed or changed to make the complexity go away?

**YAGNI** - **Y**ou **A**in’t **G**onna **N**eed **I**t - This is the answer to much introduced complexity. We force complexity upon ourselves by being so good at asking "But what if...?" Instead, we build for the problems we have today, not for the problems we think we might have someday. None of us are fortune tellers. We’re mostly wrong.

**No Punting** - No one owns an app, a system a section of code, or a problem. We don’t punt issues off to others just because we weren’t involved in the inception. Everyone works on, and fixes everything. Not knowing how something works doesn’t mean you don’t work on it or fix it, it means the opposite! We seek out every opportunity to work on things we don’t know about (and we fill in any missing documentation as we learn). Everyone on the team is full-stack. No specialists.

## Technical Priorities

1. Simple & DRY with a single responsibility
  * **Simple** - see KISS and YAGNI above.
  * **DRY** - **D**on’t **R**epeat **Y**ourself - there’s no place for copy and paste. It inevitably bites you in the ass.
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

Living these priorities requires hard thinking about the technical problems we face. Code that works is not the goal. Code that works by embodying these priorities, and the appropriate tradeoffs between them, is the goal.

## Architectural Priorities

We build services, not monoliths. Services compose in architecturally sound ways to deliver services to layers higher up the stack.

Libraries are better than micro-frameworks, which are better than monolithic frameworks.

Observers are better than Pub/Sub, which is better than Queued Data Pipelines, which are better than Synchronous Services. Observers and queues are more robust than synchronous services (eg. HTTP) and should be prefered where appropriate.

We use the right language for the job. We are a polyglot team. We prefer polyglot developers who have learned how to efficiently learn new things.

The right language for the job is generally obvious due to a language feature or library implemented in the language that makes the job a much easier match for our [technical priorities](#technical-priorities). The job becomes much simpler, or more robust, or easier to test, etc.

If the job is general purpose utility computing with no language feature or library advantages, we use [Clojure and ClojureScript](#tools) as our default.

## Process Priorities

Production code gets tested. Tests run in [continuous integration (CI)](https://en.wikipedia.org/wiki/Continuous_integration). Not all code needs all kinds of tests (unit, integration, system, acceptance, ...). Pragmatism still rules and strict [test-driven development (TDD)](https://en.wikipedia.org/wiki/Test-driven_development) is not important to us (we prefer our own version of TDD, thinking-driven development), but automated test development is a core skill of all our developers and test automation is a key consideration in code reviews.

All services do their best to trap and report errors. Error handling and alerting is a key consideration in code reviews.

Deployment is automated. It’s not done until it ships with automated deployment. It ships as soon as possible. Risk is mitigated by shipping small changes  incrementally.

All developers write documentation. Documentation is part of code review: did the appropriate docs get created and updated for the addition/change? 

Documentation is well-organized. Even when good docs exist, if the person that needs them doesn’t know where they are or that they exist, they do no good. 

Code is documented. Comments are good! It’s always critical to comment on why this code is doing what it’s doing. It’s also important to comment on what the code is doing, when that’s not completely obvious.

## Coding Guidelines

We're much more interested in ensuring code follows the values and priorities described above than that it follows any particular coding standard. Code formatting is just the small tip of a very large iceberg; poorly formatted code can be programmatically tidied, but poorly done thinking can't be programatically rethought. That being said, here are some code guidelines we look for:

* Do your best to conform to the coding style that's already there in the repository... That means we like it.
* Use 2 soft spaces for indentation for white space irrelevant code/data. Use 4 soft spaces for white space relevant code/data.
* Don't leave trailing spaces after lines.
* Don't leave trailing new lines at the end of files.
* Write comments. Comments are good.
* Write tests. Tests are good.
* Don't submit über pull requests, keep your changes focused and atomic.
* Submit pull requests to the `dev` branch of repos.
* Have fun!

## Tools

If the job is general purpose utility computing with no language feature or library advantages, we use Clojure and ClojureScript as our default.

### Clojure

[Clojure](http://clojure.org/) is a Lisp that targets the [Java Virtual Machine (JVM)](https://en.wikipedia.org/wiki/Java_virtual_machine).

Tutorials:

* [Clojure From the Ground Up](http://aphyr.com/posts/302-clojure-from-the-ground-up-basic-types)
* [Clojure for the Brave and True](http://www.braveclojure.com/)
* [Clojure Koans](http://www.4clojure.com/)

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
* [The Clojrue Toolbox](http://www.clojure-toolbox.com/)

Other:

* [Leiningen](http://leiningen.org/) - declarative dependency management and build tool (via plugins)
* [Boot](https://github.com/boot-clj/boot) - functional build and dependency management tool

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

### Om

[Om](https://github.com/omcljs/om) is a ClojureScript interface to [React](http://facebook.github.io/react/).

Tutorials:

* [Zero to Om](https://blog.stephanbehnke.com/zero-to-om/)
* [Om Basic Tutorial](https://github.com/omcljs/om/wiki/Basic-Tutorial)
* [Om Intermediate Tutorial](https://github.com/omcljs/om/wiki/Intermediate-Tutorial)
* [Building a Card Game with Om](http://www.railslove.com/stories/my-way-into-clojure-building-a-card-game-with-om-part-1)

Other:

* [Om Cookbook](https://github.com/omcljs/om-cookbook)
* [core.async](https://github.com/clojure/core.async) - library for async communication, often used in ClojureScript ([reference](https://clojure.github.io/core.async/))
* [ClojureScript, Om and core.async](http://elbenshira.com/blog/trifecta-clojurescript-om-coreasync/)
* [Om Tools](https://github.com/Prismatic/om-tools/blob/master/README.md) - library of general-purpose tools for building applications with Om


## Code of Conduct

The [code of conduct](https://github.com/open-company/developer-onboarding/blob/master/CODE_OF_CONDUCT.md) will be followed at all times.

## License

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />"<span xmlns:dct="http://purl.org/dc/terms/" href="http://purl.org/dc/dcmitype/Text" property="dct:title" rel="dct:type">Developer Onboarding</span>" by <a xmlns:cc="http://creativecommons.org/ns#" href="http://opencompany.io/" property="cc:attributionName" rel="cc:attributionURL">Transparency, LLC</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.

Copyright © 2015 Transparency, LLC