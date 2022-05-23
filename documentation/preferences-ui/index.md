---
layout: documentation
id: documentation
title: Adding a Preferences UI
---
## Adding a Preferences UI

### Sparkle 2

These instructions below are for adding preferences in a Cocoa nib and assume you have a Preferences nib with its `File's Owner` being set to your Preferences controller class (e.g, a `NSWindowController` subclass). The controller class should have an `updater` property set to your application's `SPUUpdater`, which may be passed from your application's delegate.

#### Enable automatic checking

* Drag a check button from the Library to your document.
* Set the title to something like "Automatically check for updates".
* Select the check button, and under the Bindings tab, select Value.
* Check the Bind to: check button, choose `File's Owner` from the popup, and set the Model Key Path to `updater.automaticallyChecksForUpdates`.

#### Update check interval

* Drag a popup button from the Library to your document.
* Set the titles of the menu items to e.g. "Hourly", "Daily", "Weekly", "Monthly".
* Set the tags of the menu items to the corresponding times in seconds, e.g. 3600, 86400, 604800, 2629800.
* Select the popup button, and under the Bindings tab, select Selected Tag.
* Check the Bind to: check button, choose `File's Owner` from the popup, and set the Model Key Path to `updater.updateCheckInterval`.
* Select Enabled, check the Bind to: check button, choose `File's Owner` from the popup, and set the Model Key Path to `updater.automaticallyChecksForUpdates`.

#### Other preferences

Follow directions similar to [Enable automatic checking](#enable-automatic-checking) to bind a check button to `updater.sendsSystemProfile` or `updater.automaticallyDownloadsUpdates`. See [customization](/documentation/customization/#infoplist-settings) for details on the available keys.

---

### Sparkle 1

These instructions below are for adding preferences in a Cocoa nib and assume you have a Preferences nib with its `File's Owner` being set to your Preferences controller class (e.g, a `NSWindowController` subclass). The controller class should have an `updater` property set to your application's `SUUpdater`, which may be passed from your application's delegate.

#### Enable automatic checking

* Drag a check button from the Library to your document.
* Set the title to something like "Automatically check for updates".
* Select the check button, and under the Bindings tab, select Value.
* Check the Bind to: check button, choose `File's Owner` from the popup, and set the Model Key Path to `updater.automaticallyChecksForUpdates`.

#### Update check interval

* Drag a popup button from the Library to your document.
* Set the titles of the menu items to e.g. "Hourly", "Daily", "Weekly", "Monthly".
* Set the tags of the menu items to the corresponding times in seconds, e.g. 3600, 86400, 604800, 2629800.
* Select the popup button, and under the Bindings tab, select Selected Tag.
* Check the Bind to: check button, choose `File's Owner` from the popup, and set the Model Key Path to `updater.updateCheckInterval`.
* Select Enabled, check the Bind to: check button, choose `File's Owner` from the popup, and set the Model Key Path to `updater.automaticallyChecksForUpdates`.

#### Other preferences

Follow directions similar to [Enable automatic checking](#enable-automatic-checking). to bind a check button to `sendsSystemProfile` or `automaticallyDownloadsUpdates`. See [customization](/documentation/customization/#infoplist-settings) for details on the available keys.

#### Preferences for non-app bundles

These directions do not work for non-app bundles, as the updater you add to the nib will be the `sharedUpdater` for the application bundle. To be able to bind to the updater for your bundle, you can add the following accessor to your preferences controller (the owner of the nib):

```objc
- (SUUpdater *)updater {
    return [SUUpdater updaterForBundle:[NSBundle bundleForClass:[self class]]];
}
```

Then just bind the controls to the File's Owner, and start the Model Key Path with updater., e.g. updater.automaticallyChecksForUpdates.

---

### Watch out for preference caching

macOS caches plist files in `~/Library/Preferences`, so don't edit them directly. If you want to tweak these files for testing (e.g. change last update check date), use the `defaults` command.
