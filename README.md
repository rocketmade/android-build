
##1. Make sure you include the Fabric maven repo, and the Fabric gradle to the classpath

_build.gradle_
```gradle
buildscript {
    repositories {
        maven { url 'https://maven.fabric.io/public' }
    }

    dependencies {
        classpath 'io.fabric.tools:gradle:1.+'
    }
}
```

##2. Add the Fabric plugin AFTER the com.android.application plugin

_build.gradle_
```gradle
apply plugin: 'io.fabric'
```

##3. Add the Fabric Maven repository below the apply plugin command

_build.gradle_
```gradle
repositories {
    maven { url 'https://maven.fabric.io/public' }
}

```

##4. Add the release notes, and the name of the distribution group

_build.gradle_
```gradle
def releaseNotes = 'git log --pretty=format:"%s" --since "yesterday"'.execute([], project.rootDir).text.trim()
ext {
    betaDistributionReleaseNotes = releaseNotes
    betaDistributionGroupAliases = FABRIC_GROUP_ALIAS
}
```

##5. Add the Crashlytics sdk to the gradle dependencies

_build.gradle_
```gradle
compile('com.crashlytics.sdk.android:crashlytics:+@aar') {
    transitive = true;
}
```

##6. Create a Fabric.properties file in the app/ directory, include the info:

_fabric.properties_
```ini
apiSecret=YOUR_BUILD_SECRET
apiKey=YOUR_API_KEY
```

_AndroidManifest.xml_
##7. Include the API key in your AndroidManifest.xml

```xml
<meta-data
            android:name="io.fabric.ApiKey"
            android:value="API_KEY"/>

```

##8. Start Crashlytics in your Application subclass onCreate

_MyApplication.java_
```java
private void setupFabric() {
    // Set up Crashlytics, disabled for debug builds
    Crashlytics crashlyticsKit = new Crashlytics.Builder()
            .core(new CrashlyticsCore.Builder().disabled(BuildConfig.DEBUG).build())
            .build();

    // Initialize Fabric with the debug-disabled crashlytics.
    Fabric.with(this, crashlyticsKit);
}
```
