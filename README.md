Run `gradle assembleDist --warning-mode all`

Results in:
```
The configuration :my-library:runtimeJars was resolved without accessing the project in a safe manner.  This may happen when a configuration is resolved from a thread not managed by Gradle or from a different project.  See https://docs.gradle.org/5.1/userguide/troubleshooting_dependency_resolution.html#configuration_resolution_constraints for more details. This behaviour has been deprecated and is scheduled to be removed in Gradle 6.0.
The configuration :my-library:runtimeClasspath was resolved without accessing the project in a safe manner.  This may happen when a configuration is resolved from a thread not managed by Gradle or from a different project.  See https://docs.gradle.org/5.1/userguide/troubleshooting_dependency_resolution.html#configuration_resolution_constraints for more details. This behaviour has been deprecated and is scheduled to be removed in Gradle 6.0.
```

What's the correct way to resolve this?
This problem seems to occur even if there is no runtimeJars or providedCompile configuration, and instead
simply referring to `from configurations.runtimeClasspath` from the root project,
or, the contents of a subproject's distribution from the parent project.

The goal is to create a distribution artifact that contains the jars of a subproject as well as their runtime
dependencies.

For example:

```
my-distribution.tar
|-my-library
  |-lib
    |-my-library.jar
    |-guava.jar
|-my-library-2
  |-lib
    |-my-library-2.jar
    |-guava.jar
```