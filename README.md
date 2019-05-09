# p2composite-bintray-example
An example of how to create an Eclipse p2 Composite update site during the build and publish it on Bintray.

This is described in this post:

http://www.lorenzobettini.it/2016/02/publish-an-eclipse-p2-composite-repository-on-bintray/

## Bintray Credentials

Given the following:
- `$BINTRAY_USER` is a bintray username
- `$BINTRAY_APIKEY` is the bintray apikey authorizing `$BINTRAY_USER` to publish to bintray
- `$BINTRAY_ORG` is the bintray organization that `$BINTRAY_USER` is authorized to publish to
- `$BINTRAY_REPO` is the bintray repo in the `$BINTRAY_ORG` that `$BINTRAY_USER` is authorized to publish to

Create `~/.m2/settings.xml`, replacing `$BINTRAY_USER` and `$BINTRAY_APIKEY`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                      http://maven.apache.org/xsd/settings-1.0.0.xsd/"> 
    <profiles>
        <profile>
            <id>release-composite</id>
            <activation>
                    <activeByDefault>false</activeByDefault>
            </activation>
            <properties>
                <bintray.user>$BINTRAY_USER</bintray.user>
                <bintray.apikey>$BINTRAY_APIKEY</bintray.apikey>
            </properties>
        </profile>
    </profiles>
</settings>
```

- In [p2composite.example.site/pom.xml]:

  Either explicitly the value of several properties or provide them on the command line as indicated.

## Bintray upload vs. publish

Bintray makes a distinction between [uploading vs publishing files](https://bintray.com/docs/usermanual/uploads/uploads_uploads.html).
This can be specified on the command line with an additional argument or set in the pom.xml:

- `-Dbintray.publish=0` uploads the files only. 

  These can be subsequently published via the bintray web UI or with the [jfrog cli](https://www.jfrog.com/getcli/)
  
- `-Dbintray.publish=1` uploads & publishes the files.

## Useful commands

- Set a version

    ```bash
    cd p2composite.example.tycho
    mvn org.eclipse.tycho:tycho-versions-plugin:set-version -DnewVersion=1.2.0
    ```

  This will update the version information in all 4 projects (tycho, plugin, feature, site)
  for all the relevant files: `pom.xml`, `META-INF/MANIFEST.MF` and `feature.xml`.

- Clean the build artifacts

    ```bash
    cd p2composite.example.tycho
    mvn clean
    ```
    
- Make a clean build of a new release & upload to bintray without publishing

    ```bash
    cd p2composite.example.tycho
    mvn clean verify -Prelease-composite -Dbintray.owner=... -Dbintray.repo=... -Dbintray.package=... -Dbintray.publish=0
    
    ```

- Make a clean build of a new release & upload+publish to bintray


    ```bash
    cd p2composite.example.tycho
    mvn clean verify -Prelease-composite -Dbintray.owner=... -Dbintray.repo=... -Dbintray.package=... -Dbintray.publish=1
    ```


