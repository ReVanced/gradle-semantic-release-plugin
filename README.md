# A semantic-release plugin for Gradle project

Yet another gradle-semantic-release-plugin that invokes Gradle wrapper script to release.

[![Build](https://github.com/KengoTODA/gradle-semantic-release-plugin/actions/workflows/build.yml/badge.svg)](https://github.com/KengoTODA/gradle-semantic-release-plugin/actions/workflows/build.yml)
[![npm](https://badgen.net/npm/v/gradle-semantic-release-plugin)](https://www.npmjs.com/package/gradle-semantic-release-plugin)
[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)
[![semantic-release: angular](https://img.shields.io/badge/semantic--release-angular-e10079?logo=semantic-release)](https://github.com/semantic-release/semantic-release)

# About this fork

This repository is a fork of [gradle-semantic-release-plugin](https://github.com/KengoTODA/gradle-semantic-release-plugin) to make it practically work with [multi-semantic-release](https://github.com/anolilab/semantic-release/blob/main/packages/multi-semantic-release/README.md). Specifically, it allows specifying the Gradle binary path (Useful when the plugin is ran in a Gradle module as a root directory) using the environment variable `GRADLE_BIN` (Useful when the plugin is ran in a Gradle module as a root directory).

Goals of this fork:

- Feature parity with upstream
- Support multi-semantic-release

Nongoals of this fork:

- New features that do not contribute to the goal of this fork
- Bug fixes that do not contribute to the goal of this fork

# Precondition

To apply this semantic-release plugin, you need to fulfill the following preconditions:

1. Your Gradle project should manage its version by `gradle.properties` (not by `build.gradle` nor `build.gradle.kts`).
2. Your Gradle project should have an executable Gradle wrapper script at the project root directory.
3. Your CI environment should run on Linux environment that can run `./gradlew`.
4. Your Gradle project should use [Maven Publish Plugin](https://docs.gradle.org/current/userguide/publishing_maven.html), [Legacy publishing](https://docs.gradle.org/current/userguide/artifact_management.html), [Gradle Artifactory Plugin](https://www.jfrog.com/confluence/display/RTF/Gradle+Artifactory+Plugin), [Gradle Nexus Publish Plugin](https://github.com/gradle-nexus/publish-plugin/), or [Plugin Publishing Plugin](https://docs.gradle.org/current/userguide/publishing_gradle_plugins.html) to publish artifact.

# Procedure to install

## Install semantic-release

Follow [install guide](https://semantic-release.gitbook.io/semantic-release/usage/installation) and [CI configuration guide](https://semantic-release.gitbook.io/semantic-release/usage/ci-configuration) described in the semantic-release official document.

To manage version of toolset, we recommend you to have a `package.json` in your project root directory. Manage both of `semantic-release` and its plugins as `devDependencies`.

Refer [the spotbugs-gradle-plugin project](https://github.com/spotbugs/spotbugs-gradle-plugin) as a working example.

## Configure `@semantic-release/git`

This plugin updates `gradle.properties` to bump up project version. If you want to keep the version in this file updated, configure `@semantic-release/git` to commit changes. You can configure your `package.json` like below:

```json
  "release": {
    "plugins": [
      "gradle-semantic-release-plugin",
      [
        "@semantic-release/git",
        {
          "assets": [
            "gradle.properties"
          ]
        }
      ]
    ]
  },
```

# Gradle Properties

## Publish Properties

Users can specify an API key and secret by supplying the following environment variables:

1. `GRADLE_PUBLISH_KEY=my-key`
    - This will get translated to `-Pgradle.publish.key=my-key` 
2. `GRADLE_PUBLISH_SECRET=my-secret`
    - This will get translated to `-Pgradle.publish.secret=my-secret`

Possible usage in `build.gradle.kts`

```kotlin
publishing {
  repositories {
      maven {
          name = "OrgRepo"
          url = uri("https://maven.pkg.github.com/org/repo")
          credentials {
              username = project.properties["gradle.publish.key"].toString()
              password = project.properties["gradle.publish.secret"].toString()
          }
      }
  }
}
```

# FAQ

## How it's different with the [@tschulte/gradle-semantic-release-plugin](https://github.com/tschulte/gradle-semantic-release-plugin)?

That is a Gradle plugin implemented by Java. It can use Gradle's feature and ecosystem. However, it emulates `semantic-release` and cannot use other semantic-release plugin at the same time.

Our plugin is a semantic-release plugin. It can work with other plugin implemented on node.js, but it just invokes Gradle and cannot handle so complex requirements by own.

## Copyright

Copyright (c) 2019-2024 Kengo TODA
