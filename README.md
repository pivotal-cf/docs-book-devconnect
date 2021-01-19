#  Developer Connect

**Note:** This documentation repo is currently not in use. This repo will only be used if development continues on the Developer Connect product.

## About this Repo

* This book repo is for building the **edge** version of the Developer Connect content, that is, content for the next unreleased version of the product.

* The content repo is in **[pivotalcf/docs-devconnect](https://github.com/pivotal-cf/docs-devconnect/)**.

* The production version of this doc is built as part of the devconnect book: **pivotalcf/docs-book-devconnect**.

* The `master_middleman` folder contains the templates used for publishing.

## The Docs Toolchain

The Cloud Foundry documentation is written in markdown and published using the [Bookbinder gem](http://github.com/pivotal-cf/docs-bookbinder) to generate the documentation as a web application with [Middleman](http://middlemanapp.com/).

Bookbinder enables us to single-source docs content for multiple contexts.

So, for example, the docs for the Cloud Foundry command-line tool (cf CLI) exist in a single repo and are published to all three documentation sets:

* [Pivotal Platform, Pivotal Platform Services, Pivotal Platform Partner Services, and Pivotal Platform Add-ons](http://docs.pivotal.io)
* [run.pivotal](http://docs.run.pivotal.io)
* [Open Source](http://docs.cloudfoundry.org)

## Contributing to the Documentation

The docs team prefers to receive documentation contributions as pull requests rather than having engineering teams push directly to the docs repos.
This gives us a chance to review the changes for consistency and understand the new content.

If you are planning to initiate a large documentation effort, please coordinate with the docs team in advance to make sure we're not going to step on each other.
You can reach the docs team by email at [cf-docs@pivotal.io](mailto:cf-docs@pivotal.io).

If you are trying to figure out where a particular bit of information should live, please reach out and ask.
We're happy to help you ensure information goes to the right place.

Note that content often lives in more than one context.
Please make sure that any contribution you offer takes these multiple contexts into account.
So, for example, if you are working on docs that are also used in for open source, please do not refer to Pivotal's commercial offerings.

## Submitting a Pull Request

This is a Bookbinder project. See [its README](https://github.com/pivotal-cf/bookbinder/blob/master/README.md) for instructions on how edits are made.

## Continuous Integration Technical Details

We deploy this documentation as an app using Concourse pipelines, at https://p-concourse.wings.cf-app.com/teams/system-team-docs-docs-1-88aa/pipelines/cf-current?groups=pcfservices

Credential for our pipeline are stored in LastPass.
Tarballs of the builds are stored on Amazon S3.
Use the creds stored in LastPass to log in.

The CI status can be monitored via the [Checkman](https://github.com/cppforlife/checkman) application (Mac only).

Once installed, configure Checkman to be aware of the CI builds by putting the following in a file called `~/Checkman/bookbinder`:

    PCF Bind: concourse.check https://pubtools.ci.cf-app.com REPLACE-WITH-USERNAME REPLACE-WITH-PASSWORD cf-current pcfservices

## Deployment Details

Staging and Production are both on run.pivotal.io:

- [Staging](http://cf-p1-docs-staging.cfapps.io/)
- [Production](http://cf-p1-docs-prod.cfapps.io/)

See the config.yml for details.

## Notes on the config.yml

Template variables for the Pivotal Platform docs are stored in a separate YML file, `template_variables.yml`, stored in the config folder.

The "sections" section of the `config.yml` file is organized as follows:

- First "sections" section contains all repos for Pivotal Platform docs in the cloudfoundry org, in alphabetical order.
- Next "sections" section contains all repos for Pivotal Platform docs in the pivotal-cf org, in alphabetical order.
- Next "sections" section contains the repo where we store product PDFs
- Next "sections" section contains all repos for non-Pivotal Platform docs in the pivotal-cf org, in alphabetical order.
- Next "sections" section contains all repos for non-Pivotal Platform docs in the other orgs, in alphabetical order.

## Determine Content Repos and Branches of a Book

The `config.yml` defines the content repos for each book.
The `config.yml` file of each book contains the list of content repos and branches that appear in the doc set.
In the `config.yml` file, each content repo is specified in the `repository` subsection.
This subsection includes an optional `ref` key-value pair which defines the branch of the content repo the book uses.

Make sure that you are adding your content to the correct branches of the content repos.

To determine which branch of a content repo a book version uses:

1. Confirm that you are on the correct book branch. For example, the currently published doc might be on the `master` branch
or on the branch corresponding to its version number.

2. Open the `config.yml` file.

3. Search for the name of the content repo, for example, `docs-cloudfoundry-concepts`.

4. Review the `repository` subsection for the content repo. If there is no `ref:` tag, then the repo uses the master branch. If there is a `ref` key-value pair, it specifies the branch name of the content repo. For example,

  ```
  - repository:
    name: cloudfoundry/docs-cloudfoundry-concepts
    ref: '225'
  ```

  `ref: '225'` refers to the 225 branch of the docs-cloudfoundry-concepts repo. Cloud Foundry v225 is associated with the PCF v1.6 release.
