buildscript {
    repositories {
        // ...
        maven { url 'https://plugins.gradle.org/m2/' } // Gradle Plugin Portal
    }
    dependencies {
        // ...
        // OneSignal-Gradle-Plugin
        classpath 'com.google.gms:google-services:4.0.1'
        classpath 'gradle.plugin.com.onesignal:onesignal-gradle-plugin:[0.12.6, 0.99.99]'
    }
}

apply plugin: 'com.onesignal.androidsdk.onesignal-gradle-plugin'
def localProperties = new Properties()
def localPropertiesFile = rootProject.file('local.properties')
if (localPropertiesFile.exists()) {
    localPropertiesFile.withReader('UTF-8') { reader ->
        localProperties.load(reader)
    }
}

def flutterRoot = localProperties.getProperty('flutter.sdk')
if (flutterRoot == null) {
    throw new GradleException("Flutter SDK not found. Define location with flutter.sdk in the local.properties file.")
}

def flutterVersionCode = localProperties.getProperty('flutter.versionCode')
if (flutterVersionCode == null) {
    flutterVersionCode = '1'
}

def flutterVersionName = localProperties.getProperty('flutter.versionName')
if (flutterVersionName == null) {
    flutterVersionName = '1.0'
}

apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'com.google.gms.google-services'
apply from: "$flutterRoot/packages/flutter_tools/gradle/flutter.gradle"
//apply plugin: 'com.onesignal.androidsdk.onesignal-gradle-plugin'

def keystorePropertiesFile = rootProject.file("key.properties")
def keystoreProperties = new Properties()
keystoreProperties.load(new FileInputStream(keystorePropertiesFile))


android {
    compileSdkVersion 31

    sourceSets {
        main.java.srcDirs += 'src/main/kotlin'
    }

    lintOptions {
        disable 'InvalidPackage'
    }

    defaultConfig {
        // TODO: Specify your own unifileHashesque Application ID (https://developer.android.com/studio/build/application-id.html).
        applicationId "entregadores.helpersapp.com.br"
        minSdkVersion 21
        targetSdkVersion 30
        versionCode flutterVersionCode.toInteger()
        versionName flutterVersionName
    }

     signingConfigs{
         release {
             keyAlias keystoreProperties['keyAlias']
             keyPassword keystoreProperties['keyPassword']
             storeFile file(keystoreProperties['storeFile'])
             storePassword keystoreProperties['storePassword']
         }
     }
 
    buildTypes {
        release {
            // TODO: Add your own signing config for the release build.
            // Signing with the debug keys for now, so `flutter run --release` works.
            signingConfig signingConfigs.release
            //  signingConfig signingConfigs.release
            minifyEnabled true
            useProguard true
        }
    }
}

flutter {
    source '../..'
}

dependencies {
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
    implementation platform('com.google.firebase:firebase-bom:28.4.1')
    implementation 'com.google.firebase:firebase-analytics'
}
