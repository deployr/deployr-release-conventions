DeployR Release Conventions 
===========================

**@@ Work in Progress @@**

The goal of this document is to layout git conventions and best practices that will assist DeployR maintainers with creating a new product release through Github.  Through convention and process this document aims to give our user-base the confidence we are releasing code with high quality and structure.

- [Release checklist](#user-content-release-checklist)
- [Release steps](#user-content-release-steps)
  - [Semantic versioning](#user-content-semantic-versioning)
  - [Tagging](#user-content-tagging)
  - [Changelog](#user-content-changelog)
    - [Generating CHANGELOG.md](#user-content-generating-changelogmd)
  - [Repository wiki](#user-content-repository-wiki)
    - [Template](#user-content-template)
  - [Marking release](#user-content-marking-release)
- [Format of the commit message](#user-content-format-of-the-commit-message)
  - [Subject line](#user-content-subject-line)
  - [Message body](#user-content-message-body)
  - [Message footer](#user-content-breaking-changes)
    - [Breaking changes](#user-content-breaking-changes)
    - [Referencing issues](#user-content-referencing-issues)
  - [Examples](#user-content-examples)

# Release checklist

These instructions are to assist the DeployR maintainers with creating a new product release through git/Github:

1. Create the next milestone
2. Move any open issues from the current release to the next milestone
3. Close the current milestone
4. Commit and push the deployr/{REPOSITORY} release and [tag](#user-content-tagging) it

  ```
  $ git commit -a -m 'bump to vX.Y.Z'
  $ git push origin master
  $ git tag -a vX.Y.Z -m 'Version vX.Y.Z'
  $ git push --tags origin vX.Y.Z
  ```
  
5. Update version fields in packages/npm/Maven to next version
6. Build [CHANGELOG.md](#user-content-generating-changelogmd)
7. Update the [Repository wiki](#user-content-repository-wiki) with a link to the release's `CHANGELOG.md`
8. Publish the tagged [release](#user-content-marking-release) as official

# Release steps

There are **five** steps for releasing:  versioning, tagging, creating a changelog, updating the repository wiki, and marking a release.

## Semantic Versioning

For the official Semantic Version documentation head to [semver.org](http://semver.org/). This is a brief introduction and does not cover all parts of semantic versioning.

### Format

A Semantic Version will be in the format {major}.{minor}.{patch}-{tag}

Example:
 
  `v7.3.5`

  `v7.3.5-beta-0`

  `v7.3.5-beta-2`

  `v7.3.5-beta-3`

#### Prerelease Identifiers

An additional (optional) string will be append to the value of the version string as a prerelease identifier.

A possible prerelease identifier for _v7.3.5_ could be: 

  `v7.3.5-alpha-0`
  
  `v7.3.5-beta-2`

  `v7.3.5-rc-1`

## Tagging

When the state of the release branch is ready to become a real release, some actions need to be carried out. 

- The release branch is merged into master (since every commit on master is a new release by definition). 
- The commit on master must be tagged for easy future reference to this historical version. 

To _tag_ master:

1. Use tags to mark commits with version numbers:

   ```
   $ git tag -a v7.3.0 -m 'Version 7.3.0'
   ```

2. We have to then _push tags upstream_ because this is not done by default:

   ```
   $ git push --tags
   ```

This produces a version string of the following format: 

```
v7.3.0-0-123456789123456789dsdfsfsf
^      ^  ^
|      |  |
|      |  SHA of HEAD
|      |
|      number of commits since last tag
|
last tag
```

## Changelog

Changelogs are important in order to understand how previous versions are different from the current release. All changelogs will be generated by a script at the end of a release. At the very least, a skeleton changelog will be emitted. Editing a changelog before the actual release is permitted. 

### Generating CHANGELOG.md

The generated `CHANGELOG.md` will contain three sections:

- New features

  New features in this release:

  ```
  $ git log <last release> HEAD --grep feat
  ```

- Bug fixes

  ```
  $ git log <last release> HEAD --grep fix
  ```

- Breaking changes

  ```
  $ git log <last release> HEAD --grep BREAKING
  ```

## Repository wiki

Each repository should have a [Github wiki](https://help.github.com/articles/adding-wiki-pages-via-the-online-interface/) that follows the [template](#template) below. The repository's wiki is where you can maintain a living document that is relevant to the people who are interested in contributing code or feedback. To avoid duplication, each wiki should offload any documentation to the official user guides and API docs that are hosted on [deployr.revolutionanalytics.com](http://deployr.revolutionanalytics.com).

Each repository should take full advantage of Github wikis and include the following pages linked to from the _wiki home_  page:

- `Development-Schedule.md`

  This page provides details on DeployR's development schedule, including code freeze dates and target dates for preview and stable releases. These dates are subject to change.

- `Ongoing-Development-Discussions.md`

  This page is for tracking current design proposals and code reviews. Usually, design proposals take the form of  gists, and code reviews pull requests, though that's not always the case.

- `Sprint {SPRINT_NUMBER} Tickets`

  This page is generated by Github using the following URL in your markdown:
  
  ```HTML
   <a https://github.com/deployr/{YOUR_REPOSITORY}/issues?
    direction=desc&amp;
    labels=&amp;
    milestone={SPRINT_NUMBER}&amp;
    page=1&amp;
    sort=created&amp;
    state=open">Sprint {SPRINT_NUMBER} Tickets
    </a>
  ```
  
### Template

```
# Welcome to the {YOUR_REPOSITORY} development wiki!

{YOUR_REPOSITORY}'s official user guides and API docs are hosted on 
http://deployr.revolutionanalytics.com. This wiki is where we maintain 
a living document that is relevant to the people who are interested in 
contributing code or feedback.

# Active Development on {YOUR_REPOSITORY}

- Link (Development-Schedule.md) --> Development Schedule - Code freeze dates and important milestones.
- Link (Ongoing-Development-Discussions.md) --> Ongoing Development Discussions
- Link (Github query) --> Sprint {SPRINT_NUMBER} Tickets - Active issues earmarked for the next release.

# Current Release

## {YOUR_REPOSITORY} {SEMANTIC_VERSION}

- Link --> GitHub release notes page
- Link --> {YOUR_REPOSITORY} {SEMANTIC_VERSION} Change History Rollup
- Link --> Zip dist file
- Link --> Maven Central repository 'ArtifactId' (when appropriate)
- Link --> npm module (when appropriate)

## Past Releases

- Link --> {YOUR_REPOSITORY} {SEMANTIC_VERSION} Change History Rollup
- Link --> {YOUR_REPOSITORY} {SEMANTIC_VERSION} Change History Rollup
- Link --> {YOUR_REPOSITORY} {SEMANTIC_VERSION} Change History Rollup
...

# Future

TDB

# Get Involved (optional)

- Link --> Deprecation Policy (TBD)
- Link --> Contribution Standards (TBD)

```

## Marking Release

Once the master branch has been [tagged](#tagging) with the appropriate [release points](#semantic-versioning), we can begin to prepare that tag for a release. Marking the tagged historical version as a _release_ is done from Github using their [Releases](https://github.com/blog/1547-release-your-software) workflow.

In Github:

1. Click the _releases_ link at the top of the repository.
2. Click the _Create a new release_ button.
3. Choose the _Tag version_ from the drop-down list (or input field if this is the first release).
4. Give the release a title based on the [semantic version](#semantic-versioning). _Release titles_ should adhere to the [release title conventions](#release-title-convention).
5. Add release notes. _Release notes_ should adhere to the [release notes conventions](#release-notes-convention).
6. If appropriate, attached the release _binaries_ by dragging and dropping them to the page or via the file chooser.
7. Finally, publish the tagged release by clicking the _Publish release_ button.

### Release Title Convention

The title of a release should only contain its corresponding [semantic version](Semantic Versioning) minus the **v** from the version string. In addition, if the semantic version contains a [prerelease identifier](#prerelease-identifiers) replace the delimited **-** character with a space.

For example, the _release title_ for version:

- _v7.3.3_  ----> `7.3.3`
- _v7.3.3-beta-1_  ----> `7.3.3 beta 1`

### Release Notes Convention

Release notes should be brief and contain the following:

1. A _Release Announcement_ link. This link will point to the official marketing announcement.
2. A _Change History Rollup_ link. See the [change log](#changelog) section for more information on how to create it.
3. A note on how to get a hold of the release via git:
  `Github: git checkout v7.3.3-beta-1`
4. The attached binary zip of the release build (download this for local deployments)
 
 
## Format of the Commit Message

```
{type}({scope}): {subject}
<BLANK LINE>
{body}
<BLANK LINE>
{footer}
```

Any line of the commit message cannot be longer than 80 characters, with the subject line limited to 70 characters. This allows the message to be easier to read on github as well as in various git tools.

### Subject Line
The subject line contains a succinct description of the change to the logic.

The allowed {types} are as follows:

- feat (feature)
- fix (bug fix)
- docs (documentation)
- style (formatting)
- refactor (refactoring)
- test (adding missing tests)
- chore (maintenance)

The {scope} can be anything specifying the location of the commit change e.g. the controller, the client, the logger, ect. 

The {subject} needs to use imperative, present tense: “change”, not “changed” nor “changes”. The first letter should not be capitalized, and there is no dot (.) at the end.

### Message Body
Just like the {subject}, the message {body} needs to be in the present tense, and includes the motivation for the change, as well as a contrast with the previous behavior.

### Message Footer

This section is where we annotate any breaking changes or closing defects.

#### Breaking Changes

All breaking changes need to be mentioned in the footer with the description of the change, the justification behind the change and any migration notes required. For example:

 > BREAKING CHANGE: The API interface `pipeline()` has been changed to `batch()`.

#### Referencing issues

Closed bugs should be listed on a separate line in the footer prefixed with the `closes` keyword, for example:

`closes #54321`

Or in the case of multiple issues:

`closes #54321, #9876, #12345`

## Examples

- **feat (feature)**
  
  feat($browser): onUrlChange event (popstate/hashchange/polling)
  
  Added new event to $browser:
    - forward popstate event if available
    - forward hashchange event if popstate not available
    - do polling when neither popstate nor hashchange available

  BREAKING CHANGE: $browser.onHashChange, which was removed (use onUrlChange instead)

- **fix (bug fix)**

  fix($compile): couple of unit tests for IE9

  Older IEs serialize html uppercased, but IE9 does not...
  Would be better to expect case insensitive, unfortunately jasmine does
  not allow to user regexps for throw expectations.

  Closes #392
  
  BREAKING CHANGE: foo.bar api, foo.baz should be used instead

- **docs (documentation)**

  docs(guide): updated docs

  Couple of typos fixed:
  - indentation
  - missing brace

- **style (formatting)**

  style(controller): Whitespace cleanup.

- **refactor (refactoring)**
 
  refactor(controller): Broke public function foo() up into smaller private functions.

  public function foo() internally calls:
  - bar()
  - baz()

- **test (adding missing tests)**

  test(controller): Added new test spec for authentication and cookies.

  Cookie check:
  - before
  - after
  
- **chore (maintenance)**

# RRO Release Guidelines

> Status: Working Draft

## Main Branches

### master

We will consider origin/master to be the main branch where the source code of HEAD always reflects a production-ready state. As a result, each time when changes are merged back into master, this is a new production release.

### dev

Parallel  to mater is dev. The dev branch is intended for the next release and is where all nightly builds are done from.

Once an RRO release is identified and dev is stable and ready to begin the release process, all changes in dev need to 'make their way' to master and then tagged with a release number. The process of how dev is merged to master and tagged is outlined below.

![branching workflow](/assets/branch-workflow.png)

##### Figure 1. Basic branching workflow.

__@NOTE__ – dev* should never be merged directly to master*

## Reinforcement Branches

### Release 

Supports the preparation of a new production release. Last minute minor bug fixes and changes are appropriate for this branch. Once a release branch is created the dev branch is open for changes intended for the next release.

### Hotfix 

Supports post release changes such as critical bug fixes that have been targeted and deemed mandatory for the current live release.

Unlike master* and dev* these branches are temporary and have a limited lifespan. 
They will eventually be removed.

## Branching Rules

### Naming convention

- Release branches

  release-v{R MAJOR}.{R MINOR}.{R PATCH}

  Examples:

  release-v3.2.0
  release-v3.2.1

- Hotfix branches

  hotfix-v{R MAJOR}.{R MINOR}.{R PATCH}+{RRO PATCH COUNT}

  Examples:

  hotfix-v3.2.0+1
  hotfix-v3.2.0+2
  hotfix-v3.2.1+1
  hotfix-v3.2.1+2 
  hotfix-v3.2.1+3

### Originating branch (Important)

- Release branches

  Branched off from dev

- Hotfix branches

  Branched off from master

### Merging back

- Release branches

  Must merge back to both master and dev

- Hotfix branches

  Must merge back to both master and dev

__@NOTE__  - Typically any changes made in these branches are merged back into both dev and master however there may be situations where this rule is ignored. For example, a defect corrected in a hotfix branch is now obsoleted in dev for the next release. 

### Lifecycle

1. Creating

- Release branches

```
$ git checkout -b release-v3.2.0 dev
```

- Hotfix branches

```
$ git checkout -b hotfix-v3.2.0+1 master
```

2. Finishing

When finished, the defect or change needs to be merged back into master and dev. This done in order to safeguard that the defector change is included in the next release as well.

### Merge to master

- Release branches

```
$ git checkout master
$ git merge --no-ff release-v3.2.0 
$ git tag -a v3.2.0 -m 'Version v3.2.0'
$ git push
```

- Hotfix branches

```
$ git checkout master
$ git merge --no-ff hotfix-v3.2.0+1 
$ git tag -a v3.2.0+1 -m 'Version v3.2.0+1'
$ git push
```

### Merge to dev

@NOTE - There is a good possibility that merge conflicts will occur during this step
since dev is evolving in parallel for the next release. If conflicts are present, fix, verify,  and commit the resolution.

- Release branches

```
$ git checkout dev
$ git merge --no-ff release-v3.2.0 
$ git push
```

- Hotfix branches

```
$ git checkout dev
$ git merge --no-ff hotfix-v3.2.0+1 
$ git push
```

### Removing

Once the hotfix or release branches are no longer needed they can be removed. Remember these branches are temporary and should be deleted at some point to keep the repository clean and tidy. The decision of when to remove these branches is up to you.

Example:

```
$ git branch -d release-v3.2.0
```

```
$ git branch -d hotfix-v3.2.0+1
```

## RRO Github Release

Once the master branch has been tagged with the appropriate release points, we  can begin to prepare that tag for a release. Marking the tagged historical version as a release is done from Github using their Releases Workflow.

In Github:

1. Click the ‘releases’ link at the top of the repository.

2. Click the ‘Create a new release’ button.

3. Choose the Tag version from the drop-down list 

4. Give the release a title based on the semantic version. Release titles shouldadhere to the [release title conventions](#release-title-convention).

5. Add release notes. Release notes should adhere to the [release notes conventions](#release-notes-convention).

6. If appropriate, attached the release binaries by dragging and dropping them to the page or via the file chooser. (Not appropriate for RRO since the download bundles are served up from the MRAN site).

7. Finally, publish the tagged release by clicking the ‘Publish release’ button.

### Release Title Convention

The title of a release should only contain its corresponding semantic version minus the “v” from the version string. In addition, if the semantic version contains a prerelease identifier replace the delimited - character with a space.

For example, the release title for version:

- v3.2.0   ----> RRO 3.2.0
- v3.2.0+1 ----> RRO 3.2.0+1

### Release Notes Convention

Release notes should be brief and contain the following:

1. A Release Announcement link. This link will point to the official marketing announcement.

2. A Change History Rollup link. See the change log section for more information on how to create it.

3. A note on how to get a hold of the release via git: 

   Example:

   ```
   $ git checkout v3.2.0
   ```

4. The attached binary zip of the release build (download this for local deployments)
(Not appropriate for RRO since the download bundles are served up from the MRAN site).

### Change log

Changelogs are important in order to understand how previous versions are different from the current release. A script at the end of a release will generate all changelogs. At the very least, a skeleton changelog will be emitted. Editing a changelog before the actual release is permitted.

__Generating CHANGELOG.md__

The generated CHANGELOG.md will contain three sections:

1. New features

   New features in this release:

   ```
   $ git log <last release> HEAD --grep feat
   ```

2. Bug fixes

   ```
   $ git log <last release> HEAD --grep fix
   ```

3. Breaking changes

   ```
   $ git log <last release> HEAD --grep BREAKING 
   ```

__@Note__ - Until someone has the time to build this script in Jenkins to generate a changelog the Github [compare API](https://help.github.com/articles/comparing-commits-across-time/) can be used and displayed as a link in the Github release notes:

Example (changes from v3.2.0 – to – v3.2.1):

```
[v3.2.0...v3.2.1](https://github.com/RevolutionAnalytics/RRO/compare/v3.2.0...v3.2.1)
```

