# p2composite-bintray-example
An example of how to create an Eclipse p2 Composite update site during the build and publish it on Bintray.

This is described in this post:

http://www.lorenzobettini.it/2016/02/publish-an-eclipse-p2-composite-repository-on-bintray/

To set a version:

```bash
cd p2composite.example.tycho
mvn org.eclipse.tycho:tycho-versions-plugin:set-version -DnewVersion=1.2.0
```

This will update the version information in all 4 projects (tycho, plugin, feature, site)
for all the relevant files: `pom.xml`, `META-INF/MANIFEST.MF` and `feature.xml`.

