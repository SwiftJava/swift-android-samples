## Samples for the SwiftAndroid toolchain.

Requires a build of the latest Android toolchain downloadable [here](http://johnholdsworth.com/android_toolchain.tgz).
The gradle plugin used to build is now part of the toolchain which is installed by running `swift-install/setup,sh`.
On Linux, make sure the `usr/bin` driectory of the toolchain you want to use is in your path when you
run this script.The phone must be api 21 aka Android v5+ aka Lollipop or better (I used an LG K4.)
The example app now uses SQLite which requires Beta III of the toolchain released 27/08/2017.

To create a new application, decide on a pair of interfaces to connect to and from your Swift
code and place them in a [Java Source](https://github.com/SwiftJava/swift-android-samples/blob/master/swifthello/src/main/java/com/jh/SwiftHelloBinding.java).
Use the command `./genswift.sh` in the [SwiftJava Project](https://github.com/SwiftJava/SwiftJava)
to generate Swift (& Java) sources to include in your application or adapt the
[genhello.sh](https://github.com/SwiftJava/SwiftJava/blob/master/genhello.sh) script.
If you only use interfaces/protocols, your app's only
[Package.swift](https://github.com/SwiftJava/swift-android-samples/blob/master/swifthello/src/main/swift/Package.swift)
dependency should be the core JNI interfacing code [java_swift](https://github.com/SwiftJava/java_swift).

This example is coded to work with version 4 of the toolchain which has some additional requirements
to work around requirements of the Swift port of Foundation. The cache directory used by web operations
needs to be setup in the enironment variable "TMPDIR". This would usually be the value of
Context.getCacheDir().getPath() from the java side. In addition, to be able to use SSL you
need to add a [CARoot info file](http://curl.haxx.se/docs/caextract.html) to the application's
raw resources and copy it to this cache directory to be picked up by Foundation as follows:

    setenv("URLSessionCertificateAuthorityInfoFile", cacheDir! + "/cacert.pem", 1)
    setenv("TMPDIR", cacheDir!, 1)

If you don't want peer validation you have the following option (not recommended at all)

    setenv("URLSessionCertificateAuthorityInfoFile", “INSECURE_SSL_NO_VERIFY”, 1)

##

Simple demo of Swift code accessed over JNI.

To build, setup the Gradle plugin, then run `./gradlew installDebug`

This demo is licensed under the Creative Commons CC0 license:
do whatever you want.

