---
title: "Keeping Up With the Dependencies"
seoTitle: "The Importance of Keeping Dependencies Up to Date"
seoDescription: "Outdated dependencies can cause compatibility issues, security vulnerabilities, and other problems in Android projects."
datePublished: Wed Mar 29 2023 15:38:44 GMT+0000 (Coordinated Universal Time)
cuid: clftuqhra000709lj4virfiyv
slug: keeping-up-with-the-dependencies
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/MxrkWAV6k7M/upload/fdae306a1a824aa11f3ed9171ea13942.jpeg
tags: android-development, gradle, android

---

Dependencies are a fundamental aspect of Android app development, allowing you to incorporate functionality and features from third-party libraries into your projects. However, dependencies are not static - they are frequently updated by their creators to improve performance, fix bugs, and address security vulnerabilities.

Despite the importance of keeping dependencies up to date, many Android projects suffer from outdated dependencies that can lead to compatibility issues, security vulnerabilities, and other problems. In this blog post, we'll explore why it's crucial to often update the dependencies of an Android project and how to do it effectively.

---

At this point, we all know that maintaining a software project is not an easy task. Most of the time we are focusing only on the new features of the product and not on the tech debt we are leaving behind. Updating dependencies can be a time-consuming process, especially if there are a lot of dependencies to update or if the updates require significant changes to the codebase. You may not have the time or resources available to devote to this process, particularly if you are working on a tight schedule. They can sometimes introduce compatibility issues with other parts of the codebase or with other dependencies which can be particularly problematic if the project is complex or has been in development for a long time, as there may be many different dependencies and components to consider.

There is always the risk of breaking changes. Dependency updates can sometimes introduce breaking changes, which can cause the app to stop working or behave unexpectedly. You may be hesitant to update dependencies if you are concerned about the potential impact of these changes on the app's functionality. And in some cases, different dependencies may require different versions of the same library, which can lead to conflicts and make it difficult to update dependencies without introducing other issues.

And last but not least, you may not be aware of the importance of keeping dependencies up to date, or may not have the necessary knowledge or tools to perform updates effectively, and this is what this blog post is trying to solve.

# Why should we care?

There are several advantages to regularly updating the dependencies of an Android app during development. Dependency updates may include bug fixes that address issues discovered by other developers or users, and they can also include performance improvements, such as faster load times or reduced memory usage. So updating your dependencies can help you avoid these issues and ensure your app runs smoothly and stays up to date with the latest optimizations.

Some dependency updates also often include security patches that address known vulnerabilities in the libraries or frameworks being used. By keeping dependencies up to date, you can reduce the risk of security breaches and keep user data safe.

One of my favorite reasons is that developers are constantly improving their libraries and frameworks, and updating your dependencies can give you access to new features and functionality. This can help you stay competitive and provide your users with new and improved experiences. Plus, keeping your dependencies up to date can also make it easier to maintain your app over time. By staying current with the latest versions of the libraries and frameworks you use, you can avoid compatibility issues and make it easier to upgrade to new versions of Android in the future, for example.

# Recommendations for Updating Dependencies

## Avoid Dynamic Versions

Dynamic versions refer to declaring a dependency using a version range or wildcard, such as "1.+", instead of a specific version number. While this approach can be convenient in some cases, it can also lead to unexpected behavior if a new version of the dependency is released that introduces breaking changes or incompatible features.

For example, if you declare a dependency using a version range of "1.+", the build system will automatically select the latest version of the dependency that matches that range. However, if a new version is released that introduces breaking changes, the app may fail to build or behave unexpectedly at runtime, even if no changes have been made to the app's code.

By declaring dependencies using specific version numbers, you can ensure that the app is built using a known, stable version of each dependency. This can help avoid compatibility issues and make it easier to troubleshoot issues if they arise. Additionally, declaring specific versions can help ensure that the app behaves consistently across different builds and environments, making it easier to test and deploy.

## Use Version Catalogs

> A *version catalog* is a list of dependencies, represented as dependency coordinates, that a user can pick from when declaring dependencies in a build script.

A [Gradle version catalog](https://docs.gradle.org/current/userguide/platforms.html) is a file that contains information about available dependencies and plugins that can be used in your project. It's essentially a centralized repository that tracks the latest versions of dependencies and plugins, making it easier to keep them up-to-date in your Android app.

The benefits of using a Gradle version catalog are many:

* For each catalog, Gradle generates *type-safe accessors* so that you can easily add dependencies with autocompletion in the IDE.
    
* Each catalog is visible to all projects of a build. It is a central place to declare a version of a dependency and to make sure that a change to that version applies to every subproject.
    
* Catalogs can declare [dependency bundles](https://docs.gradle.org/current/userguide/platforms.html#sec:dependency-bundles), which are "groups of dependencies" that are commonly used together.
    
* Catalogs can separate the group and name of a dependency from its actual version and use [version references](https://docs.gradle.org/current/userguide/platforms.html#sec:common-version-numbers) instead, making it possible to share a version declaration between multiple dependencies.
    
* Is supported by services like Renovate Bot, which helps automate the process of updating the dependencies. More info on this is below.
    

The easiest way of adding a version catalog to your project is by adding a `libs.versions.toml` file under your root `gradle` directory.

The syntax is simple enough:

```bash
// gradle/libs.versions.toml

[versions]
detekt = "1.22.0"
exoplayer = "2.18.4"

[libraries]
exoplayer-core = { module = "com.google.android.exoplayer:exoplayer-core", version.ref = "exoplayer" }
exoplayer-hls = { module = "com.google.android.exoplayer:exoplayer-hls", version.ref = "exoplayer" }
exoplayer-ui = { module = "com.google.android.exoplayer:exoplayer-ui", version.ref = "exoplayer" }

[bundles]
exoplayer = ["exoplayer.core", "exoplayer.hls", "exoplayer.ui"]

[plugins]
detekt = { id = "io.gitlab.arturbosch.detekt", version.ref = "detekt" }
```

Then you can use the dependency coordinate from the version catalog in a build script file:

```kotlin
// app/build.gradle.kts
dependencies {
    implementation(libs.exoplayer.core)
}
```

## Check for Deprecated API Calls

Before updating a dependency, it is important to review the release notes for the new version to identify any breaking changes or deprecations that may affect the app. This can help you prepare for any necessary changes to the app's codebase.

There are also several tools available that can help identify deprecated API calls or other issues that may be introduced by a dependency update. For example, the [Android Lint tool](https://developer.android.com/studio/write/lint#overview) can highlight deprecated API calls, while the Gradle build system can help identify version conflicts and other issues. I would also recommend enabling the `allWarningsAsErrors` property of the [Kotlin Gradle Plugin](https://kotlinlang.org/docs/gradle-compiler-options.html#attributes-common-to-jvm-js-and-js-dce). This will help you to notice any new warnings that a dependency update can introduce and suppress the warning with more intention if needed.

Once the app code has been updated to address any breaking changes or deprecations, it is important to thoroughly test the app to ensure that it still functions as expected. This may involve testing individual features, conducting automated or manual testing, or using tools like unit tests or integration tests.

And of course, when making changes to the app's codebase to address breaking changes or deprecations, it is important to use version control tools like Git to track and manage changes. This can help ensure that changes are properly documented and can be rolled back if necessary.

## Use Automation Tools

Automation tools like \[Renovate Bot\]([https://github.com/renovatebot/renovate](https://github.com/renovatebot/renovate)) can help simplify the process of updating dependencies by automatically monitoring for updates, reviewing release notes, and creating pull requests to update outdated dependencies.

Renovate Bot is an open-source tool that can automatically monitor, track, and update the dependencies of a project. It uses GitHub or GitLab to track the dependencies in a repository and automatically opens pull requests to update outdated dependencies.

When an outdated dependency is identified, Renovate Bot will automatically create a pull request with the necessary changes to update the dependency to the latest version. The pull request will include links to relevant release notes or documentation.

Renovate Bot can be customized to fit the specific needs of a project, including configuring which dependencies to update, how often to check for updates, and how to handle conflicts or compatibility issues. It can also be set up to automatically merge pull requests that pass automated tests, further streamlining the process of updating dependencies.

# Takeaways

Updating dependencies is a critical aspect of Android app development, ensuring that apps remain performant, secure, and compatible with different versions of Android and other dependencies. By following best practices like avoiding dynamic versions, checking for deprecated API calls, using version catalogs, and leveraging automation tools, you can simplify the process of updating dependencies and ensure that your Android apps remain up-to-date and secure.