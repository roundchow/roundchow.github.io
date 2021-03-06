---
title:  "正在进行时"
subtitle: "Work in Progress"
author: "wu"
avatar: "img/authors/tigerCat.jpg"
image: "img/f.jpg"
date:   2018-11-06 03:00:00
---

进行时 Work in Progress

## Key Points

· [Babel is a JavaScript compiler](https://babeljs.io/docs/en/)

<pre>

Babel is a toolchain that is mainly used to convert ECMAScript 2015+ code into backwards compatible version of JavaScript in current and older browsers or environments. 

Here are the main things Babel can do for you: 

(1) Transform syntax
(2) Pollyfill features that are misssing in your target environment
(3) Source code tansformations
(4) And more ... 

</pre>

· [ESLint is an open source JavaScript linting utility](https://eslint.org/docs/about/)

<pre>

JavaScript, being a dynamic and loosely-typed language, is especially prone to developer error. Without the benefit of a compilation process, JavaScript code is typically executed in order to find syntax or other errors. Linting tools like ESLint allow developers to discover problems with their JavaScript code without executing it.

The primary reason ESLint was created was to allow developers to create their own linting rules. ESLint is designed to have all rules completely pluggable. The default rules are written just like any plugin rules would be. They can all follow the same pattern, both for the rules themselves as well as tests. While ESLint will ship with some built-in rules to make it useful from the start, you'll be abe to dynamically load rules at any point in time. 

Philosophy

Everything is pluggable:

(1) Rule API is used both by bundled and custom rules
(2) Formatter API is used both by bundled and custom formatters
(3) Additional rules and formatters can be specified at runtime
(4) Rules and formatters don’t have to be bundled to be used

Every rule:

(1) Is standalone
(2) Can be turned off or on (nothing can be deemed “too important to turn off”)
(3) Can be set to a warning or error individually

Additionally:

(1) Rules are “agenda free” - ESLint does not promote any particular coding style
(2) Any bundled rules are generalizable

The project:

(1) Values documentation and clear communication
(2) Is as transparent as possible
Believes in the importance of testing

</pre>

· [npm is the package manager for JavaScript and the world's largest software registry.](https://docs.npmjs.com/getting-started/what-is-npm)

<pre>

npm opens up an entire world of JavaScript talent for you and your team. It's the world's largest software registry, with approximately 3 billion downloads per week. The registry contains over 600,000 packages (building blocks of code). Open-source developers from every continent use npm to share and borrow packages, and many organizations use npm to manage private development as well.

npm consists of three distinct components:

(1) the website
(2) the Command Line Interface (CLI)
(3) the registry

Use the website to discover packages, set up profiles, and manage other aspects of your npm experience.

The CLI runs from a terminal. This is how most developers interact with npm.

The registry is a large public database of JavaScript software and the meta-information surrounding it.

</pre>

· [.gitlab-ci.yml is used by GitLab Runner to manage your project's jobs.](https://docs.gitlab.com/ee/ci/quick_start/README.html)

<pre>

GitLab offers a continuous integration service. If you add a .gitlab-ci.yml file to the root directory of your repository, and configure your GitLab project to use a Runner, then each commit or push triggers your CI pipeline.

The .gitlab-ci.yml file tells the GitLab runner what to do. By default it runs a pipeline with three stages: build, test, and deploy. You don’t need to use all three stages; stages with no jobs are simply ignored.

If everything runs OK (no non-zero return values), you’ll get a nice green checkmark associated with the commit. This makes it easy to see whether a commit caused any of the tests to fail before you even look at the code.

Most projects use GitLab’s CI service to run the test suite so that developers get immediate feedback if they broke something.

There’s a growing trend to use continuous delivery and continuous deployment to automatically deploy tested code to staging and production environments.

So in brief, the steps needed to have a working CI can be summed up to:

(1) Add .gitlab-ci.yml to the root directory of your repository
(2) Configure a Runner

From there on, on every push to your Git repository, the Runner will automagically start the pipeline and the pipeline will appear under the project’s Pipelines page.

</pre>

· [Webpack is a static module bundler for modern JavaScript applications. ](https://webpack.github.io/)

<pre>

When webpack processes your application, it internally builds a dependency graph which maps every module your project needs and generates one or more bundles.

Since version 4.0.0, webpack does not require a configuration file to bundle your project, nevertheless it is incredibly configurable to better fit your needs.

To get started you only need to understand its Core Concepts:

(1) Entry
(2) Output
(3) Loaders
(4) Plugins
(5) Mode

For a better understanding of the ideas behind module bundlers and how they work under the hood consult these resources:

(1) Manually Bundling an Application
(2) Live Coding a Simple Module Bundler
(3) Detailed Explanation of a Simple Module Bundler

</pre>

· 正在进行时 Work in Progress - Loi Wu -

<div class="scale"><img src="img/hugkiss.jpg"  alt="λanguage" /></div>



