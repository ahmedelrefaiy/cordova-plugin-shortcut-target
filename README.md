# Cordova Plugin for adding a shortcut to an existing iOS Project dynamically

This plugin extends your existing xcode project by parsing and modifying the project.pbxproj file using [cordova-node-xcode](https://github.com/apache/cordova-node-xcode). The shortcut extension will be added to the XCode-Project everytime a `cordova platform add ios` is done.

## Usage

### 1. First of all you have to create a Intents Extension yourself using XCode (`Editor > Add target > Intents Extension`)

* Fill in the fields, making note of the following:
 * Remember the name of the Shortcut
 * Remember the last part of the bundle identifier (the suffix)
 * Shortcut bundle id will be like your host app then `.your_custom_name`. Example: `app.any.main.Shortcuts`
* Enable the `App Groups` entitlement (`Targets > Select your shortcut > Capabilities`) and name your group: `group.<Bundle-ID of your host app>` (you can use the group to share NSUserDefaults between the Shortcut and the main App). _Note that you have to add this to your provisioning profile_
* When done implementing your shortcut copy the the main folder of it to `www` folder. [you can move it at any other place, but you need to specify this place in plugin parameters].
* If you need to add custom build settings you can use a xcconfig file, the script will add it to the project
* Every file that is not a `.swift`, `.h`, `.m`, `.plist`, `.entitlements`, `.xcconfig` or `.storyboard` file will be added as a resource file to the project (images, fonts, etc.)

### 2. Install the plugin
* `cordova plugin add https://github.com/ahmedelrefaiy/cordova-plugin-shortcut-target --save`
* This will not modify anything yet because the hooks only run `after_platform_add`
* You can add variables to your `config.xml` in order to change some of the settings:

| Variable | Default | Description |
|-|-|-|
|WIDGET_PATH| `/www` | Path to the folder that contains your shortcut folder relative to the project root |
|WIDGET_NAME| <Name of main project> Shortcut | Name of your Shortcut |
|WIDGET_BUNDLE_SUFFIX| Shortcuts | The last part of the Shortcut bundle id |
|ALWAYS_EMBED_SWIFT_STANDARD_LIBRARIES| YES | You might have to turn this off (change to NO) if you use other swift based plugins (such as cordova-plugin-geofence) |
|SWIFT_VERSION| '3.0' | The version of Swift that your widget uses |
|Build_TEAM_ID|  | Developer account teamId |
|PROVISIONING_PROFILE_UUID|  | UUID of provisioning profile for singing |

This can be done either manually in the config.xml after installing the plugin, or be done through the CLI.

#### Example:

In the config.xml

```
<plugin name="cordova-plugin-shortcut-target" src="https://github.com/ahmedelrefaiy/cordova-plugin-shortcut-target">
            <variable name="WIDGET_NAME" value="Shortcuts" />
            <variable name="WIDGET_BUNDLE_SUFFIX" value="Shortcuts" />
            <variable name="SWIFT_VERSION" value="5.0" />
            <variable name="ALWAYS_EMBED_SWIFT_STANDARD_LIBRARIES" value="YES" />
            <variable name="Build_TEAM_ID" value="XXXXXXXXXX"/>
            <variable name="PROVISIONING_PROFILE_UUID" value="XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"/>
   </plugin>
```

### Acknowledgements

Thanks to [DavidStrausz](https://github.com/DavidStrausz/cordova-plugin-today-widget) for his plugin `cordova-plugin-today-widget`. Our plugin was built from his one.