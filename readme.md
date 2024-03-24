# Mod Gradle Scripts

This repository contains Gradle scripts that contain shared code between projects.
This is intended to reduce the build script boilerplate in individual projects.

## `mod.gradle`

This script simplifies the configuration of a mod development environment including uploads to distribution platforms.

This script supports:

- Fabric Loom `1.4+`
- Quilt Loom `1.4+`
- NeoGradle `7.0+`
- ForgeGradle `6.0+`

Gradle version `8.5+` is required.

### Usage

In your mods `build.gradle` apply all needed plugins like **Shadow** and **Fabric Loom** and then add the following line:

```groovy
apply from: "https://raw.githubusercontent.com/henkelmax/mod-gradle-scripts/${mod_gradle_script_version}/mod.gradle"
```

### Properties

| Property                     | Description                                                                                                                                                       | Type                          | Default Value                      | Possible Values                                  | Example       |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- | ---------------------------------- | ------------------------------------------------ | ------------- |
| `minecraft_version`          | The Minecraft version                                                                                                                                             | `String`                      | *This field is required to be set* |                                                  | `1.20.4`      |
| `mod_id`                     | The mod ID                                                                                                                                                        | `String`                      | *This field is required to be set* |                                                  | `examplemod`  |
| `mod_version`                | The version of the mod                                                                                                                                            | `String`                      | *This field is required to be set* |                                                  | `1.0.0`       |
| `mod_display_name`           | The display name of the mod                                                                                                                                       | `String`                      | *This field is required to be set* |                                                  | `Example Mod` |
| `java_version`               | The required Java version of the mod                                                                                                                              | `String`/`int`                | `17`                               |                                                  |               |
| `mod_loader`                 | The mod loader                                                                                                                                                    | `String`                      | `fabric`                           | `fabric`, `quilt`, `neoforge`, `forge`, `bukkit` |               |
| `mod_authors`                | The mod authors                                                                                                                                                   | Comma separated `String` list | `Max Henkel`                       |                                                  |               |
| `minecraft_user_name_prefix` | The in-game  player name prefix                                                                                                                                   | `String`                      | `henkelmax`                        |                                                  |               |
| `include_mixins`             | Whether Mixin should be added to the mod loader. This only applies to mod loaders that have the abilities to use Mixins and don't already include them by default | `boolean`                     | `false`                            |                                                  |               |
| `use_mixins`                 | Whether the mod uses Mixins. This is only needed for mod loaders that need the mixins.json registered in the buildscript                                          | `boolean`                     | `false`                            |                                                  |               |
| `included_projects`          | The subprojects that should be included in the project                                                                                                            | Comma separated `String` list | *Empty list*                       |                                                  | `:common`     |
| `use_shadow`                 | Whether to use shadow                                                                                                                                             | `boolean`                     | `true`                             |                                                  |               |
| `enable_configbuilder`       | Whether config builder should be included in the project. Also shades the library if `use_shadow` is enabled                                                      | `boolean`                     | `false`                            |                                                  |               |


**If `use_shadow` is active**

The Gradle plugin `com.github.johnrengelman.shadow` needs to applied before applying this script.


**If `mod_loader` `fabric` is used**

The Gradle plugin `fabric-loom` needs to applied before applying this script.

| Property                      | Description                                                                                                | Type                          | Default Value | Possible Values | Example                                  |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------- | ----------------------------- | ------------- | --------------- | ---------------------------------------- |
| `fabric_loader_version`       | The Fabric Loader version                                                                                  | `String`                      | `0.15.1`      |                 |                                          |
| `included_fabric_api_modules` | The Fabric API modules that should be included in the project                                              | Comma separated `String` list | *Empty list*  |                 | `fabric-api-base, fabric-command-api-v2` |
| `import_fabric_api`           | Whether Fabric API should be available in development and the Minecraft runtime                            | `boolean`                     | `false`       |                 |                                          |
| `fabric_api_version`          | The Fabric API version. Required if `included_fabric_api_modules` is set or `import_fabric_api` is enabled | `String`                      |               |                 | `0.91.3+1.20.4`                          |
| `enable_accesswideners`       | If access wideners should be enabled. Uses `${mod_id}.accesswidener` as name                               | `boolean`                     | `false`       |                 |                                          |
| `add_quilt_supported_tag`     | If quilt should be marked as supported when uploading the mod                                              | `boolean`                     | `true`        |                 |                                          |


**If `mod_loader` `quilt` is used**

The Gradle plugin `org.quiltmc.loom` needs to applied before applying this script.

| Property                | Description                                                                  | Type      | Default Value                      | Possible Values | Example  |
| ----------------------- | ---------------------------------------------------------------------------- | --------- | ---------------------------------- | --------------- | -------- |
| `quilt_loader_version`  | The Quilt Loader version                                                     | `String`  | *This field is required to be set* |                 | `0.19.0` |
| `enable_accesswideners` | If access wideners should be enabled. Uses `${mod_id}.accesswidener` as name | `boolean` | `false`                            |                 |          |


**If `mod_loader` `neoforge` is used**

The Gradle plugins `net.neoforged.gradle.userdev` and `net.neoforged.gradle.mixin` need to applied before applying this script.

| Property                    | Description                                                                             | Type      | Default Value                      | Possible Values | Example   |
| --------------------------- | --------------------------------------------------------------------------------------- | --------- | ---------------------------------- | --------------- | --------- |
| `neoforge_version`          | The NeoForge version                                                                    | `String`  | *This field is required to be set* |                 | `20.4.40` |
| `enable_accesstransformers` | If access transformers should be enabled. Uses `META-INF/accesstransformer.cfg` as path | `boolean` | `false`                            |                 |           |


**If `mod_loader` `forge` is used**

The Gradle plugin `net.minecraftforge.gradle` needs to applied before applying this script.

Additionally the plugin `org.spongepowered.mixin` needs to be applied if `include_mixins` is enabled.

| Property                             | Description                                                                             | Type      | Default Value                            | Possible Values | Example                                  |
| ------------------------------------ | --------------------------------------------------------------------------------------- | --------- | ---------------------------------------- | --------------- | ---------------------------------------- |
| `forge_version`                      | The Forge version                                                                       | `String`  | *This field is required to be set*       |                 | `48.0.1`                                 |
| `mixin_annotation_processor_version` | The version of the Mixin annotation processor                                           | `String`  | `0.8.4`                                  |                 |                                          |
| `mixin_connector_path`               | The path to the Mixin connector. Required if `use_mixins` is enabled                    | `String`  |                                          |                 | `de.maxhenkel.examplemod.MixinConnector` |
| `merge_files`                        | Whether to combine all classes and resources                                            | `boolean` | `true`                                   |                 |                                          |
| `forge_mappings_channel`             | The mappings channel                                                                    | `String`  | `official`                               |                 |                                          |
| `forge_mappings_version`             | The mappings version                                                                    | `String`  | The `minecraft_version` that was defined |                 |                                          |
| `enable_accesstransformers`          | If access transformers should be enabled. Uses `META-INF/accesstransformer.cfg` as path | `boolean` | `false`                                  |                 |                                          |


**If `mod_loader` `bukkit` is used**

| Property         | Description        | Type     | Default Value                      | Possible Values | Example                |
| ---------------- | ------------------ | -------- | ---------------------------------- | --------------- | ---------------------- |
| `bukkit_version` | The Bukkit version | `String` | *This field is required to be set* |                 | `1.20.4-R0.1-SNAPSHOT` |


**Upload related properties**

| Property                          | Description                                                                         | Type      | Default Value | Possible Values            | Example |
| --------------------------------- | ----------------------------------------------------------------------------------- | --------- | ------------- | -------------------------- | ------- |
| `upload_release_type`             | The release type of the upload                                                      | `String`  | `release`     | `alpha`, `beta`, `release` |         |
| `upload_recommended`              | If the upload is a recommended build                                                | `boolean` | `false`       |                            |         |
| `enable_curseforge_upload`        | Adds a CurseForge upload task                                                       | `boolean` | `true`        |                            |         |
| `enable_curseforge_bukkit_upload` | Adds a CurseForge Bukkit upload task (For `mod_loader`=`bukkit` only)               | `boolean` | `false`       |                            |         |
| `enable_modrinth_upload`          | Adds a Modrinth upload task                                                         | `boolean` | `true`        |                            |         |
| `enable_hangar_upload`            | Adds a Hangar upload task (For `mod_loader`=`bukkit` only)                          | `boolean` | `false`       |                            |         |
| `enable_mod_update`               | Adds a mod update task (See [this](https://github.com/henkelmax/mod-update-plugin)) | `boolean` | `true`        |                            |         |


**If `enable_curseforge_upload` is active**

You need to set the `CURSEFORGE_API_KEY` environment variable to be able to upload mods.

The Gradle plugin `com.matthewprenger.cursegradle` needs to applied before applying this script.

| Property                                  | Description                                                                                       | Type                          | Default Value                            | Possible Values | Example     |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------- | ----------------------------- | ---------------------------------------- | --------------- | ----------- |
| `curseforge_upload_id`                    | The ID of the CurseForge project                                                                  | `String`                      | *This field is required to be set*       |                 | `123456`    |
| `curseforge_upload_include_prefix`        | If the file name of the upload should include a prefix with the loader name and Minecraft version | `boolean`                     | `true`                                   |                 |             |
| `curseforge_upload_minecraft_version`     | The Minecraft version for the upload                                                              | `String`                      | The `minecraft_version` that was defined |                 | `1.20.4`    |
| `curseforge_upload_optional_dependencies` | Optional mod dependencies                                                                         | Comma separated `String` list | *Empty list*                             |                 | `jei, jade` |
| `curseforge_upload_required_dependencies` | Required mod dependencies                                                                         | Comma separated `String` list | *Empty list*                             |                 | `jei, jade` |


**If `enable_curseforge_bukkit_upload` is active**

You need to set the `CURSEFORGE_API_KEY` environment variable to be able to upload plugins.

The Gradle plugin `info.u_team.curse_gradle_uploader` needs to applied before applying this script.

| Property                                      | Description                             | Type                          | Default Value                      | Possible Values | Example                |
| --------------------------------------------- | --------------------------------------- | ----------------------------- | ---------------------------------- | --------------- | ---------------------- |
| `curseforge_bukkit_upload_id`                 | The ID of the CurseForge Bukkit project | `String`                      | *This field is required to be set* |                 | `123456`               |
| `curseforge_bukkit_upload_minecraft_versions` | Supported Minecraft versions            | Comma separated `String` list | *Empty list*                       |                 | `1.20, 1.20.1, 1.20.2` |


**If `enable_modrinth_upload` is active**

You need to set the `MODRINTH_TOKEN` environment variable to be able to upload mods.

The Gradle plugin `icom.modrinth.minotaur` needs to applied before applying this script.

| Property                                  | Description                                                                                       | Type                          | Default Value                            | Possible Values | Example          |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------- | ----------------------------- | ---------------------------------------- | --------------- | ---------------- |
| `modrinth_upload_id`                      | The ID of the Modrinth project                                                                    | `String`                      | *This field is required to be set*       |                 | `abc123`         |
| `modrinth_upload_include_prefix`          | If the file name of the upload should include a prefix with the loader name and Minecraft version | `boolean`                     | `true`                                   |                 |                  |
| `modrinth_upload_supported_loaders`       | The mod loaders that are supported                                                                | Comma separated `String` list | The `mod_loader` that was defined        |                 | `fabric, quilt`  |
| `modrinth_upload_supported_game_versions` | The supported Minecraft versions for the upload                                                   | Comma separated `String` list | The `minecraft_version` that was defined |                 | `1.20.3, 1.20.4` |
| `modrinth_upload_optional_dependencies`   | Optional mod dependencies                                                                         | Comma separated `String` list | *Empty list*                             |                 | `jei, jade`      |
| `modrinth_upload_required_dependencies`   | Required mod dependencies                                                                         | Comma separated `String` list | *Empty list*                             |                 | `jei, jade`      |


**If `enable_hangar_upload` is active**

You need to set the `HANGAR_API_KEY` environment variable to be able to upload plugins.

The Gradle plugin `io.papermc.hangar-publish-plugin` needs to applied before applying this script.

| Property                           | Description                                     | Type                          | Default Value                      | Possible Values                  | Example                       |
| ---------------------------------- | ----------------------------------------------- | ----------------------------- | ---------------------------------- | -------------------------------- | ----------------------------- |
| `hangar_upload_id`                 | The ID of the Hangar project                    | `String`                      | *This field is required to be set* |                                  | `ExampleProject`              |
| `hangar_upload_minecraft_versions` | The supported Minecraft versions for the upload | Comma separated `String` list | *Empty list*                       |                                  | `1.20.3, 1.20.4`              |
| `hangar_upload_supported_loaders`  | The supported loaders                           | Comma separated `String` list | *Empty list*                       | `PAPER`, `VELOCITY`, `WATERFALL` | `PAPER, VELOCITY`              |
| `hangar_upload_dependencies`       | The plugin dependencies                         | Comma separated `String` list | *Empty list*                       |                                  | `SimpleVoiceChat, ViaVersion` |


**If `enable_mod_update` is active**

You need to set the `MOD_UPDATE_API_KEY` environment variable to be able to upload updates.

The Gradle plugin `mod-update` needs to applied before applying this script.


## `taskutils.gradle`

Utilities to run multiple tasks consecutively.
