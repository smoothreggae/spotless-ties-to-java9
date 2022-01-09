# project to demonstrate code in Spotless that requires Java 9+
This is a simple project to demonstrate ties to Java 9+ in the bytecode for
Spotless even though Spotless is expected to work with Java 8.
[https://jira.mongodb.org/browse/JAVA-2559](This ticket) has a useful
explanation for what the underlying cause may be.

Although the project uses version `5.17.0` of the Spotless Gradle plugin, I
was able to reproduce the problem with the following versions as well:

* `5.4.0`
* `6.0.0`
* `6.1.2`

## pre-requisites
- a Java SE 8 installation
- a Java SE 11 installation
- the ability to switch between the two using the environment
  variable `JAVA_HOME`

## steps

### seeing the problem when using Java 8
- set `JAVA_HOME` to point to your Java SE 8 installation
- run `./gradlew -v` and confirm that the entry for `JVM` matches
  your Java SE 8 installation
- run `./gradlew spotlessCheck`

Here is the failure you should expect to see:

    > Task :spotlessMisc FAILED

    FAILURE: Build failed with an exception.

    * What went wrong:
    Execution failed for task ':spotlessMisc'.
    > java.nio.ByteBuffer.clear()Ljava/nio/ByteBuffer;

### seeing graceful reporting when using Java 11
- set `JAVA_HOME` to point to your Java SE 11 installation
- run `./gradlew -v` and confirm that the entry for `JVM` matches
  your Java SE 11 installation
- run `./gradlew spotlessCheck`

Here is the failure you should expect to see:

    > Task :spotlessMisc FAILED

    FAILURE: Build failed with an exception.

    * What went wrong:
    Execution failed for task ':spotlessMisc'.
    > Encoding error! Spotless uses UTF-8 by default.  At line 14 col 176:
      spa�ol  <- UTF-8
      spañol  <- windows-1252
      spañol  <- ISO-8859-1
      spa隳l ( <- Big5
      spa隳l ( <- Big5-HKSCS
      spa駉l ( <- GBK
      spa駉l ( <- GB18030
