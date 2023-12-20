# Mod Gradle Scripts

This repository contains Gradle scripts that contain shared code between projects.
This is intended to reduce the build script boilerplate in individual projects.

This script supports:

- Fabric Loom `1.4+`
- NeoGradle `7.0+`
- ForgeGradle `6.0+`


Gradle version `8.5+` is required.

## Usage

In your mods `build.gradle` apply all needed plugins like **Shadow** and **Fabric Loom** and then add the following line:

``` groovy
apply from: "https://raw.githubusercontent.com/henkelmax/mod-gradle-scripts/${mod_gradle_script_version}/mod.gradle"
```
