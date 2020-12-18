<img src="http://developer.postgresql.org/~josh/graphics/logos/elephant-64.png" />

# ARCHIVAL NOTICE

The repository is archived, and it is **no longer used**.

# Parent poms for PostgreSQL JDBC driver


[![Build Status](https://travis-ci.org/pgjdbc/pgjdbc-parent-poms.png)](https://travis-ci.org/pgjdbc/pgjdbc-parent-poms)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/org.postgresql/pgjdbc-versions/badge.svg)](https://maven-badges.herokuapp.com/maven-central/org.postgresql/pgjdbc-versions)

This repository includes Maven parent poms that are were by PostgreSQL JDBC driver REL9.4.1207..REL42.2.12.

As pgjdbc switched to Gradle, this repository is no longer used.

## Info

You probably do not need to clone/build this repository.
In order to contribute a feature / file a bug report for JDBC driver, please use [JDBC driver](http://github.com/pgjdbc/pgjdbc) main repository.

In case base dependency (e.g. `maven-compiler-plugin` version) needs to be changed, a relevant change to the `pgjdbc-parent-poms` repository should
be made and this new version should be used in main `pgjdbc` repository.

## Downloading pre-built drivers

Most people do not need to compile PgJDBC. You can download prebuilt versions of the driver 
from the [Postgresql JDBC site](http://jdbc.postgresql.org/) or using your chosen dependency management tool
(see details at [JDBC driver](http://github.com/pgjdbc/pgjdbc) )

## Build requirements

In order to build the set of parent poms, you will need the following tools:

- A git client
- A recent version of Maven (3.x)
- A JDK (any should work)

## Checking out the source code

The PgJDBC project uses git for version control. You can check out the current code by running:

    git clone https://github.com/pgjdbc/pgjdbc-parent-poms.git

This will create a pgjdbc-parent-poms directory containing the checked-out source code.

## Installing parent poms to local repository

After checking out the code you can install new poms to your local repository:

    mvn install

## Releasing a snapshot version

Git repository typically contains -SNAPSHOT versions, so you can use the following command:

    mvn deploy

## Releasing a new version

### Release via Travis

To release a branch via Travis, perform the following:

TL;DR:

    git checkout -B release/master origin/master
    git push origin release/master

1. Check if `pom.xml` includes proper `-SNAPSHOT` versions (release versions would be the ones without `-SNAPSHOT`)
1. Check if there are `RELx.y.z` tags left behind from unsuccessful release attempts
1. Push `release/master` branch to pointing to the commit you want to release

    Travis would build new version, create a tag, update `pom.xml` to the next snapshot versions, and update `master` branch accordingly.

    Note: the artifacts will not be visible in Maven Central before you manually release them.

1. Navigate to [Sonatype Nexus Repository Manager](https://oss.sonatype.org/#stagingRepositories), find staging `orgpostgresql` repository there and release it

### Manual release

Procedure:

To commit updates to version in `pom.xml` files and create a tag, issue:

    mvn release:clean release:prepare

To stage the version to maven central, issue:

    mvn release:perform

This will open staging repository for smoke testing access at https://oss.sonatype.org/content/repositories/orgpostgresql-1082/

If staged artifacts look fine, release it

    mvn nexus-staging:release -DstagingRepositoryId=orgpostgresql-1082

### In case release fails

In case release fails, the following cleanup is required:

1. Git tags. For instance: `git push origin :RELx.y.z`
1. Drop staging repository (if exists). Navigate to [Sonatype Nexus Repository Manager](https://oss.sonatype.org/#stagingRepositories), find staging `orgpostgresql` repository there and drop it

## Dependencies

`pgjdbc-parent-poms` has little to no dependencies itself. It just lists defaults to be used by core `pgjdbc` project.

## Bug reports, patches and development

PgJDBC development is carried out on the [PgJDBC mailing list](https://jdbc.postgresql.org/community/mailinglist.html) and on [GitHub](https://github.com/pgjdbc/pgjdbc).

### Bug reports

For bug reports please post on pgsql-jdbc or add a GitHub issue. If you include
additional unit tests demonstrating the issue, or self-contained runnable test
case including SQL scripts etc that shows the problem, your report is likely to
get more attention. Make sure you include appropriate details on your
environment, like your JDK version, container/appserver if any, platform,
PostgreSQL version, etc. Err on the site of excess detail if in doubt.

### Bug fixes and new features

If you've developed a patch you want to propose for inclusion in PgJDBC, feel
free to send a GitHub pull request or post the patch on the PgJDBC mailing
list.  Make sure your patch includes additional unit tests demonstrating and
testing any new features. In the case of bug fixes, where possible include a
new unit test that failed before the fix and passes after it.

For information on working with GitHub, see: http://help.github.com/articles/fork-a-repo and http://learn.github.com/p/intro.html.

### Testing

Remember to test proposed PgJDBC patches when running against older PostgreSQL
versions where possible, not just against the PostgreSQL you use yourself.

You also need to test your changes with older JDKs. PgJDBC must support JDK6
("Java 1.6") and newer. Code that is specific to a particular spec version
may use features from that version of the language. i.e. JDBC4.1 specific 
may use JDK7 features, JDBC4.2 may use JDK8 features.
Common code and JDBC4 code needs to be compiled using JDK6.

### Ideas

If you have ideas or proposed changes, please post on the mailing list or
open a detailed, specific GitHub issue.

Think about how the change would affect other users, what side effects it
might have, how practical it is to implement, what implications it would
have for standards compliance and security, etc. Include a detailed use-case
description.

Few of the PgJDBC developers have much spare time, so it's unlikely that your
idea will be picked up and implemented for you. The best way to make sure a
desired feature or improvement happens is to implement it yourself. The PgJDBC
sources are reasonably clear and they're pure Java, so it's sometimes easier
than you might expect.

## Support for IDEs

It's possible to debug and test PgJDBC with various IDEs, not just with `mvn` on
the command line. Projects aren't supplied, but it's easy to prepare them.

## <a name="commit"></a> Git Commit Guidelines

We have very precise rules over how our git commit messages can be formatted.  This leads to **more
readable messages** that are easy to follow when looking through the **project history**.  But also,
we use the git commit messages to **generate the change log**.

### Commit Message Format
Each commit message consists of a **header**, a **body** and a **footer**.  The header has a special
format that includes a **type**, and a **subject**:

```
<type>: <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

Any line of the commit message cannot be longer 100 characters! This allows the message to be easier
to read on github as well as in various git tools.

### Type
Must be one of the following:

* **feat**: A new feature
* **fix**: A bug fix
* **docs**: Documentation only changes
* **style**: Changes that do not affect the meaning of the code (white-space, formatting, missing
  semi-colons, etc)
* **refactor**: A code change that neither fixes a bug or adds a feature
* **perf**: A code change that improves performance
* **test**: Adding missing tests
* **chore**: Changes to the build process or auxiliary tools and libraries such as documentation
  generation

### Subject
The subject contains succinct description of the change:

* use the imperative, present tense: "change" not "changed" nor "changes"
* don't capitalize first letter
* no dot (.) at the end

###Body
Just as in the **subject**, use the imperative, present tense: "change" not "changed" nor "changes"
The body should include the motivation for the change and contrast this with previous behavior.

###Footer
The footer should contain any information about **Breaking Changes** and is also the place to
reference GitHub issues that this commit **Closes**.


### Sponsors

[PostgreSQL International](http://www.postgresintl.com)
