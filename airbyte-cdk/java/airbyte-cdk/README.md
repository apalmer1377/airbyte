# Developing with the Java CDK

This page will walk through the process of developing with the Java CDK.

- [Developing with the Java CDK](#developing-with-the-java-cdk)
  - [Intro to the Java CDK](#intro-to-the-java-cdk)
    - [What is included in the Java CDK?](#what-is-included-in-the-java-cdk)
    - [How is the CDK published?](#how-is-the-cdk-published)
  - [Using the Java CDK](#using-the-java-cdk)
    - [Building the CDK](#building-the-cdk)
    - [Bumping the CDK version](#bumping-the-cdk-version)
    - [Publishing the CDK](#publishing-the-cdk)
  - [Developing Connectors with the Java CDK](#developing-connectors-with-the-java-cdk)
    - [Referencing the CDK from Java connectors](#referencing-the-cdk-from-java-connectors)
    - [Developing a connector alongside the CDK](#developing-a-connector-alongside-the-cdk)
    - [Developing a connector against a pinned CDK version](#developing-a-connector-against-a-pinned-cdk-version)
  - [Common Debugging Tips](#common-debugging-tips)
  - [Changelog](#changelog)
    - [Java CDK](#java-cdk)

## Intro to the Java CDK

### What is included in the Java CDK?

The java CDK is comprised of separate modules:

- `core` - Shared classes for building connectors of all types.
- `db-sources` - Shared classes for building DB sources.
- `db-destinations` - Shared classes for building DB destinations.

Each CDK submodule may contain these elements:

- `src/main` - (Required.) The classes that will ship with the connector, providing capabilities to the connectors.
- `src/test` - (Required.) These are unit tests that run as part of every build of the CDK. They help ensure that CDK `main` code is in a healthy state.
- `src/test-integration` - (Optional.) Integration tests which provide a more extensive test of the code in `src/main`. These are not by the `build` command but are executed as part of the `integrationTest` or `integrationTestJava` Gradle tasks.
- `src/testFixtures` - (Optional.) These shared classes are exported for connectors for use in the connectors' own test implementations. Connectors will have access to these classes within their unit and integration tests, but the classes will not be shipped with connectors when they are published.

### How is the CDK published?

The CDK is published as a set of jar files sharing a version number. Every submodule generates one runtime jar for the main classes. If the submodule contains test fixtures, a second jar will be published with the test fixtures classes.

Note: Connectors do not have to manage which jars they should depend on, as this is handled automatically by the `airbyte-java-connector` plugin. See example below.

## Using the Java CDK

### Building the CDK

To build and test the Java CDK, execute the following:

```sh
./gradlew :airbyte-cdk:java:airbyte-cdk:build
```

### Bumping the CDK version

You will need to bump this version manually whenever you are making changes to code inside the CDK.

While under development, the next version number for the CDK is tracked in the file: `airbyte-cdk/java/airbyte-cdk/core/src/main/resources/version.properties`.

If the CDK is not being modified, this file will contain the most recently published version number.

### Publishing the CDK

_⚠️ These steps should only be performed after all testing and approvals are in place on the PR. ⚠️_

The CDK can be published with a GitHub Workflow and a slash command which can be run by Airbyte personnel.

To invoke via slash command (recommended), use the following syntax in a comment on the PR that contains your changes:

```bash
/publish-java-cdk                # Run with the defaults (dry-run=false, force=false)
/publish-java-cdk dry-run=true   # Run in dry-run mode (no-op)
/publish-java-cdk force=true     # Force-publish if needing to replace an already published version
```

Note:

- Remember to **document your changes** in the Changelog section below.
- After you publish the CDK, remember to toggle `useLocalCdk` back to `false` in all connectors.
- Unless you specify `force=true`, the pipeline should fail if the version you are trying to publish already exists.
- By running the publish with `dry-run=true`, you can confirm the process is working as expected, without actually publishing the changes.
- In dry-run mode, you can also view and download the jars that are generated. To do so, navigate to the job status in GitHub Actions and navigate to the 'artifacts' section.
- You can also invoke manually in the GitHub Web UI. To do so: go to `Actions` tab, select the `Publish Java CDK` workflow, and click `Run workflow`.
- You can view and administer published CDK versions here: https://admin.cloudrepo.io/repository/airbyte-public-jars/io/airbyte/airbyte-cdk
- The public endpoint for published CDK versions is here: https://airbyte.mycloudrepo.io/public/repositories/airbyte-public-jars/io/airbyte/airbyte-cdk/

## Developing Connectors with the Java CDK

### Referencing the CDK from Java connectors

You can reference the CDK in your connector's `build.gradle` file:

```groovy
plugins {
    id 'application'
    id 'airbyte-java-connector'
}

airbyteJavaConnector {
    cdkVersionRequired = '0.1.0'   // The CDK version to pin to.
    features = ['db-destinations'] // An array of CDK features to depend on.
    useLocalCdk = false            // Use 'true' to use a live reference to the 
                                   // local cdk project.
}

airbyteJavaConnector.addCdkDependencies()
```

Replace `0.1.0` with the CDK version you are working with. If you're actively developing the CDK and want to use the latest version locally, use the `useLocalCdk` flag to use the live CDK code during builds and tests.

### Developing a connector alongside the CDK

You can iterate on changes in the CDK local and test them in the connector without needing to publish the CDK changes publicly.

When modifying the CDK and a connector in the same PR or branch, please use the following steps:

1. Set the version of the CDK in `version.properties` to the next appropriate version number and add a description in the `Changelog` at the bottom of this readme file.
2. Modify your connector's build.gradle file as follows:
   1. Set `useLocalCdk` to `true` in the connector you are working on. This will ensure the connector always uses the local CDK definitions instead of the published version.
   2. Set `cdkVersionRequired` to use the new _to-be-published_ CDK version.

After the above, you can build and test your connector as usual. Gradle will automatically use the local CDK code files while you are working on the connector.

Once you are done developing and testing your CDK changes:

1. Publish the CDK using the instructions here in this readme.
2. After publishing the CDK, update the `useLocalCdk` setting by running `./gradlew :airbyte-integrations:connectors:<connector-name>:disableLocalCdkRefs`. to automatically revert `useLocalCdk` to `false`.
3. You can optionally run `./gradlew :airbyte-integrations:connectors:<connector-name>:assertNotUsingLocalCdk` to ensure that the project is not using a local CDK reference.

_Note: You can also use  `./gradlew assertNotUsingLocalCdk` or `./gradlew disableLocalCdkRefs` to run these tasks on **all** connectors simultaneously._

### Developing a connector against a pinned CDK version

You can always pin your connector to a prior stable version of the CDK, which may not match what is the latest version in the `airbyte` repo. For instance, your connector can be pinned to `0.1.1` while the latest version may be `0.2.0`.

Maven and Gradle will automatically reference the correct (pinned) version of the CDK for your connector, and you can use your local IDE to browse the prior version of the codebase that corresponds to that version.

## Common Debugging Tips

MavenLocal debugging steps:

1. Confirm local publish status by running:
   `ls -la ~/.m2/repository/io/airbyte/airbyte-cdk/*`
2. Confirm jar contents by running:
   `jar tf ~/.m2/repository/io/airbyte/airbyte-cdk/0.0.2-SNAPSHOT/airbyte-cdk-0.0.2-SNAPSHOT.jar`
3. Remove CDK artifacts from MavenLocal by running:
   `rm -rf ~/.m2/repository/io/airbyte/airbyte-cdk/*`
4. Rebuid CDK artifacts by running:
   `./gradlew :airbyte-cdk:java:airbyte-cdk:build`
   or
   `./gradlew :airbyte-cdk:java:airbyte-cdk:publishToMavenLocal`

## Changelog

### Java CDK

| Version | Date       | Pull Request                                               | Subject                               |
| :------ | :--------- | :--------------------------------------------------------- | :------------------------------------ |
| 0.1.0   | 2023-09-27 | [\#30445](https://github.com/airbytehq/airbyte/pull/30445) | First launch, including shared classes for all connectors. |
| 0.0.2   | 2023-08-21 | [\#28687](https://github.com/airbytehq/airbyte/pull/28687) | Version bump only (no other changes). |
| 0.0.1   | 2023-08-08 | [\#28687](https://github.com/airbytehq/airbyte/pull/28687) | Initial release for testing.          |
