# cljdoc for Library Authors

- [Why cljdoc?](#why-cljdoc)
- [Basic Setup](#basic-setup)
- [Articles](#articles)
	- [Auto-inferred Articles](#auto-inferred-articles)
	- [Configuring Articles](#configuring-articles)
- [More Features](#more-features)
	- [Wikilinks](#wikilinks)
	- [Intelligent Version Resolving](#intelligent-version-resolving)
	- [Offline Docs](#offline-docs)
    - [Building Docs Locally](#building-docs-locally)

## Why cljdoc?

cljdoc aims to automate as much as possible of the process of publishing documentation. This means you get to spend time on crafting great documentation while cljdoc takes care of publishing and serving them to your users.

## Basic Setup

For basic docs to be available you don't have to do anything at all. cljdoc will pick up new releases as they are published to Clojars.

**Building docs for older releases:** If you would like to build docs for a version that existed before cljdoc started building docs automatically just head to [the homepage](https://cljdoc.xyz), search for your project and pick it from the list. If we haven't built docs for it yet you'll be able to do so with the press of a button.

**Point users to your projects documentation:** You can link to cljdoc from your projects README using a plain Markdown link or the cljdoc badge:

```sh
# This will redirect to the latest released version
https://cljdoc.xyz/d/$group-id/$artifact-id/CURRENT
```

```sh
# This will show a badge with the latest released version
https://cljdoc.xyz/badge/$group-id/$artifact-id
```

In full it may look like this:

```markdown
[![cljdoc badge](https://cljdoc.xyz/badge/manifold/manifold)](https://cljdoc.xyz/d/manifold/manifold/CURRENT)
```

[![cljdoc badge](https://cljdoc.xyz/badge/manifold/manifold)](https://cljdoc.xyz/d/manifold/manifold/CURRENT)

**That's pretty much it.** Read on for some more advanced stuff that will make your docs extra awesome!

## Articles

Some libraries may want to publish additional guides, tutorials or, as they are called in cljdoc: articles.

In order to do so the following prerequisites must be met:

- Articles must be stored inside your project's Git repository 
- Your Git repository is [properly linked to in your POM](faq.md#how-do-i-set-scm-info-for-my-project). 

This allows cljdoc to retrieve files at the revision/commit the respective release was made. Those docs always being in sync will save your users a lot of trouble.

> **Note:** Articles are always read at the revision linked to the version docs are being built for. This means you cannot update docs for previous releases but are instead encouraged to cut a new release. For testing purposes you can build any `-SNAPSHOT` release which will cause cljdoc to use `master` as revision.

#### Automatic Link Rewriting

If an article refers to another article's source file via a link cljdoc will automatically rewrite that link if one of the known articles has the same source file. All links that work on GitHub should also work on cljdoc.

### Auto-inferred Articles

If your repository does not contain a `doc/cljdoc.edn` file cljdoc will try to include articles from your `doc/` or `docs/` folder automatically. 

The title is read from the file's first heading. There will be no nesting and articles will be ordered alphabetically by filename. If you don't need nesting this may be sufficient.

> **Tip:** Use filenames prefixed with digits like `01-intro.md` to easily define the order of articles.

### Configuring Articles

If you need more control you may provide a file `doc/cljdoc.edn` to specify a tree of articles.

Assuming you have a directory `doc/` in your repository as follows:

```
doc/
  getting-started.md
  installation.md
  configuration.md
```

You can integrate those files into your cljdoc build by adding a file `doc/cljdoc.edn` with the following contents:

```clojure
{:cljdoc.doc/tree [["Readme" {:file "README.md"]
                   ["Getting Started" {:file "doc/getting-started.md"}
                    ["Installation" {:file "doc/installation.md"}]]
                   ["Configuration" {:file "doc/configuration.md"}]]}
```

Which will result in the following hierarchy being shown in your docs:

```
├── Readme
├── Getting Started
│   └── Installation
└── Configuration
```

## More Features

#### Wikilinks

You can refer to other namespaces and functions inside your docstrings using `[[wikilink]]` syntax. Note that if you want to link to vars outside the current namespace you need to either fully qualify those vars or specify them relative to the current namespace. An example: if you want to link to `compojure.core/GET` from `compojure.route` you'll need to provide the wiki in one of the two forms below:

```
[[compojure.core/GET]]
[[core/GET]]
```

#### Intelligent Version Resolving

If you want to refer to namespaces, vars or similar in an article you can use `CURRENT` instead of a specific version.

- If that link is clicked while viewing the project's docs on cljdoc the version will be resolved based on the referring URL.
- If that link is clicked outside of cljdoc the version will be resolved to the latest release version.

An example linking to `reagent.core`:

https://cljdoc.xyz/d/reagent/reagent/CURRENT/api/reagent.core

#### Offline Docs

See [Offline Docs](for-users.md#offline-docs).

#### Building Docs Locally

This may be useful to test your changes without pushing new releases
to Clojars or commits to Github. See [Running cljdoc
locally](/doc/running-cljdoc-locally.md) for details.