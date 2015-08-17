# POM JSON Format

This README describes the `pom.json` format for the [VersionEye API](https://www.versioneye.com/api/). 

The VersionEye API has this endpoint to create a new project at VersionEye. Simply upload a project file via HTTP POST to this endpoint: 

```
POST /v2/projects
```

The uploaded file should have the name 'upload'. Supported project files are for example Gemfile, package.json, composer.json and many more. A full list can be found on the [VersionEye landing page](https://www.versioneye.com/). 

To update an existing project with a new project file simply upload the file via HTTP POST to this endpoint: 

```
POST /v2/projects/PROJECT_ID
```

The uploaded file should have the name 'project_file'. That will update the dependencies of an existing project with the current dependencies from the project file. 

That all works well for modern dependency managers like Bundler, NPM, Composer and Bower.IO. Dependency managers in the Java world are a little bit more complex. Many times the dependencies and their configurations are distributed over multiple files in multiple directories. That's why it's better to use a native plugin to resolve all dependencies locally and only sending the results to the API. The VersionEye API supports a general file format `pom.json` which is independently from all dependency/package managers. The `pom.json` file is JSON format which looks like that: 

```
{
    "name": "",
    "group_id": "",
    "artifact_id": "",
    "language": "", 
    "prod_type": "",
    "dependencies": [
        {
            "name": "",
            "version": "",
            "scope": ""
        }
    ]
}
```

Here is an example how it looks like with production data: 

```
{
    "name": "MySecretJdkProject",
    "group_id": "my.secret",
    "artifact_id": "project",
    "language": "Java", 
    "prod_type": "Maven2",
    "dependencies": [
        {
            "name": "junut:junit",
            "version": "4.12",
            "scope": "test"
        }, 
        {
            "name": "org.springframework:spring-core",
            "version": "4.2.0.RELEASE",
            "scope": "compile"
        }
    ]
}
```

The [VersionEye Maven Plugin](https://github.com/versioneye/versioneye_maven_plugin) is generating such a `pom.json` file and sending it to the API for creating/updating a project. [Here](https://github.com/versioneye/versioneye_maven_plugin/blob/master/src/main/java/com/versioneye/utils/JsonUtils.java#L111) you can see how the `pom.json` file gets generated. 

Please use this format for other native plugins as well. 
