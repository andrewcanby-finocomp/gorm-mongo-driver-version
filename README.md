# GORM for MongoDB dependency resolution


## Background
The documentation for Gorm for MongoDB version 7.0 says there is 
> Support for MongoDB Driver 3.10.0

At first glance I thought this meant the bundled version was updated to 3.10.0, 
but it still seems to be on 3.8.2. However - [looking at the code](https://github.com/grails/gorm-mongodb/blob/v7.0.1/gradle.properties#L7)
it seems to imply it should be 3.10.0.


## Example
This project is a vanilla Grails 4.0.4 project setup using:
1. `grails create-app --profile=rest-api mongo-version-test`
2. Adding a dependency `compile "org.grails.plugins:mongodb"`
3. Run `./gradlew assemble dependencyInsight --dependency "org.mongodb:mongodb-driver"`
 for the following output:

```
org.mongodb:mongodb-driver:3.8.2 (selected by rule)
   variant "compile" [
      org.gradle.status              = release (not requested)
      org.gradle.usage               = java-api
      org.gradle.libraryelements     = jar (compatible with: classes+resources)
      org.gradle.category            = library (not requested)

      Requested attributes not found in the selected variant:
         org.gradle.dependency.bundling = external
         org.gradle.jvm.version         = 8
   ]

org.mongodb:mongodb-driver:3.10.0 -> 3.8.2
+--- org.grails:grails-datastore-gorm-mongodb:7.0.1
|    \--- org.grails.plugins:mongodb:7.0.1
|         \--- compileClasspath (requested org.grails.plugins:mongodb)
\--- org.grails:grails-datastore-gorm-mongodb-ext:7.0.1
     \--- org.grails.plugins:mongodb:7.0.1 (*)

org.mongodb:mongodb-driver-core:3.8.2 (selected by rule)
   variant "compile" [
      org.gradle.status              = release (not requested)
      org.gradle.usage               = java-api
      org.gradle.libraryelements     = jar (compatible with: classes+resources)
      org.gradle.category            = library (not requested)

      Requested attributes not found in the selected variant:
         org.gradle.dependency.bundling = external
         org.gradle.jvm.version         = 8
   ]

org.mongodb:mongodb-driver-core:3.8.2
\--- org.mongodb:mongodb-driver:3.8.2
     +--- org.grails:grails-datastore-gorm-mongodb:7.0.1 (requested org.mongodb:mongodb-driver:3.10.0)
     |    \--- org.grails.plugins:mongodb:7.0.1
     |         \--- compileClasspath (requested org.grails.plugins:mongodb)
     \--- org.grails:grails-datastore-gorm-mongodb-ext:7.0.1 (requested org.mongodb:mongodb-driver:3.10.0)
          \--- org.grails.plugins:mongodb:7.0.1 (*)

(*) - dependencies omitted (listed previously)

A web-based, searchable dependency report is available by adding the --scan option.
```