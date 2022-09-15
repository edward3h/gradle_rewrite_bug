# Reproduce bug in Rewrite

When I run the rewriteRun task it claims to just be adding a newline to the end of `build.gradle` but it actually changes the custom install task in a way which breaks the build.

```sh
$ gw rewriteRun

> Task :rewriteRun
Validating active recipes
Parsing sources from project gradle_rewrite_bug
Plain Text :/home/edward/github/edward3h/gradle_rewrite_bug/.gitignore
Plain Text :/home/edward/github/edward3h/gradle_rewrite_bug/gradlew.bat
Plain Text :/home/edward/github/edward3h/gradle_rewrite_bug/.gitattributes
Plain Text :/home/edward/github/edward3h/gradle_rewrite_bug/gradlew
All sources parsed, running active recipes: org.openrewrite.java.cleanup.Cleanup
Changes have been made to build.gradle by:
    org.openrewrite.java.cleanup.Cleanup
        org.openrewrite.java.format.EmptyNewlineAtEndOfFile
Please review and commit the results.
```

```diff
diff --git a/build.gradle b/build.gradle
index 05f1221..e4ef94f 100644
--- a/build.gradle
+++ b/build.gradle
@@ -27,7 +27,7 @@ task install {
     dependsOn installDist
     doLast {
         ant.mkdir(dir: "${rootDir}/bin")
-        ant.symlink(resource: "${(installDist.outputs.files as List).first()}/bin/${application.applicationName}", link: "${rootDir}/bin/${application.applicationName}")
+        ant.symlink(resource: "${(installDist.outputs.files as List)installDist.outputs}bin${application////}bin${application.filesfirst}/bin/",link, link: "${rootDir}/bin/${application.applicationName}")
     }
 }
 
@@ -39,4 +39,4 @@ clean.dependsOn cleanBin
 
 rewrite {
     activeRecipe("org.openrewrite.java.cleanup.Cleanup")
-}
\ No newline at end of file
+}
```