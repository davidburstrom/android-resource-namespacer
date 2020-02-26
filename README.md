# android-resource-namespacer

Simple script that namespaces all Android R-class references where necessary, in Java and Kotlin code.

It has been verified to work with Android Gradle Plugin 3.6 and Gradle 6.1. Any other version combination of said tools may or may not work.

# Suggested usage

First ensure that your project compiles cleanly, otherwise you're in for a very confusing experience.

Also make sure your source code is under source control and has no local modifications.

Copy the `namespacer.gradle` script to the root of your Android project.

Enable namespaced R-classes for your project by adding

```
android.namespacedRClass=true
```

to the `gradle.properties` file.

Apply the namespacer script on Android (sub-)projects, e.g. add the following to the root project `build.gradle` file:

```
subprojects {
    if (hasProperty('android')) {
        apply from: rootProject.file('namespacer.gradle')
    }
}
```

Run `./gradlew namespace` on your project, then try to compile and hope for the best. You might run into the following issues:

* Since the R-classes no longer contains references to transitive resources, it is highly likely
that you'll need to add explicit dependencies on previously implicit dependencies, e.g. 'androidx' libraries
or sub-projects. You'll notice what's missing when compiling after namespacing. After adding the dependencies, run `./gradlew namespace` again.

* The script will not modify existing incorrect namespacing, so if there is a reference to e.g. `your.subproject.lib1.R.string.hello` but
the `hello` string was actually from `:your:subproject:lib2`, compilation will fail. Remove the fully qualified path and run `./gradlew namespace` again.

Once everything builds cleanly, remove the `subprojects` closure and the script and commit the changes. Ensure you keep the `android.namespacedRClass=true` entry in `gradle.properties`
otherwise this was a pointless exercise.

# Contributions

Support for other AGP versions and scenarios, as well as improvements and bug fixes are more than welcome.

# DISCLAIMER

This SOFTWARE PRODUCT is provided by THE PROVIDER "as is" and "with all faults." THE PROVIDER makes no representations or warranties of any kind concerning the safety, suitability, lack of viruses, inaccuracies, typographical errors, or other harmful components of this SOFTWARE PRODUCT. There are inherent dangers in the use of any software, and you are solely responsible for determining whether this SOFTWARE PRODUCT is compatible with your equipment and other software installed on your equipment. You are also solely responsible for the protection of your equipment and backup of your data, and THE PROVIDER will not be liable for any damages you may suffer in connection with using, modifying, or distributing this SOFTWARE PRODUCT.

