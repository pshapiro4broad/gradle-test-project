# gradle-test-project

A project to demonstrate an issue where the jib and srcclr plugins conflict. To see the error:

```sh
./gradlew jibDockerBuild
```

You should see the error:
```
> Task :service:jibDockerBuild FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':service:jibDockerBuild'.
> com.google.cloud.tools.jib.plugins.common.BuildStepsExecutionException: 'void org.apache.commons.compress.archivers.tar.TarArchiveEntry.<init>(java.nio.file.Path, java.lang.String, java.nio.file.LinkOption[])'
```

Changing srcclr so it's no longer a global plugin appears to fix jib, but then srcclr itself no longer works. After making this change, if you run srcclr by using:
```sh
SRCCLR_API_TOKEN=<<token>> ./gradlew srcclr
```

You get the error:
```
Execution failed for task ':service:srcclr'.
> com.sourceclear.engine.scan.SrcclrScanUnexpectedCondition: Conflicting property-based creators: already had implicitly discovered creator [constructor for `com.sourceclear.api.data.methods.MethodModel` (4 args), annotations: [null], encountered another: [constructor for `com.sourceclear.api.data.methods.MethodModel` (5 args), annotations: [null]
   at [Source: (String)"{"links":{"vulnerabilityDatabaseUrl":"https://sca.analysiscenter.veracode.com/vulnerability-database"},"scanId":"f308fc12-8b31-46cf-a92c-5a8715c0413d","projectName":null,"organization":null,"scanType":"repo","commitHash":null,"repoUrl":"ssh://git@github.com/pshapiro4broad/gradle-test-project.git","consoleUploadStatus":"SUCCESS","vulnMethods":false,"containerName":null,"containerTag":null,"branch":null,"components":[]}"; line: 1, column: 1]
```

Running this test requires a valid sourceclear token, so unfortuantely this step is harder to test.
