---
layout: documentation
id: documentation
title: Setting up Sparkle programmatically
---

## Create an updater programmatically

Here is an example of creating an updater in Sparkle 2 (beta) with Cocoa programmatically. It hooks up a menu item's target/action for checking for updates.

```swift
import Cocoa
import Sparkle

@NSApplicationMain
@objc class AppDelegate: NSObject, NSApplicationDelegate {
    // Hook up this menu item outlet in Interface Builder. The menu item's title is typically "Check for Updates…"
    @IBOutlet var checkForUpdatesMenuItem: NSMenuItem!
    
    let updaterController: SPUStandardUpdaterController
    
    override init() {
        // If you want to start the updater manually, pass false to startingUpdater and call .startUpdater() later
        // This is where you can also pass an updater delegate if you need one
        updaterController = SPUStandardUpdaterController(startingUpdater: true, updaterDelegate: nil, userDriverDelegate: nil)
    }
    
    func applicationDidFinishLaunching(_ notification: Notification) {
        // Hooking up the menu item's target/action to the standard updater controller does two things:
        // 1. The menu item's action is set to perform a user-initiated check for new updates
        // 2. The menu item is enabled and disabled by the updater controller depending on -[SPUUpdater canCheckForUpdates]
        checkForUpdatesMenuItem.target = updaterController
        checkForUpdatesMenuItem.action = #selector(SPUStandardUpdaterController.checkForUpdates(_:))
    }
}
```

In Sparkle 2 you can also choose to instantiate and use [SPUUpdater](/documentation/api-reference/Classes/SPUUpdater.html) directly instead of the [SPUStandardUpdaterController](/documentation/api-reference/Classes/SPUStandardUpdaterController.html) wrapper if you need more control over your user interface or what bundle to update.

If you use another UI toolkit, these are the relevant APIs in Sparkle 2 for checking for updates and handling menu item validation:

* [-[SPUStandardUpdaterController startUpdater]](/documentation/api-reference/Classes/SPUStandardUpdaterController.html#/c:objc(cs)SPUStandardUpdaterController(im)startUpdater) or [-[SPUUpdater startUpdater:]](/documentation/api-reference/Classes/SPUUpdater.html#/c:objc(cs)SPUUpdater(im)startUpdater:) for starting the updater (unless it is specified to be automatically started)
* [-[SPUStandardUpdaterController checkForUpdates:]](/documentation/api-reference/Classes/SPUStandardUpdaterController.html#/c:objc(cs)SPUStandardUpdaterController(im)checkForUpdates:) or [-[SPUUpdater checkForUpdates]](/documentation/api-reference/Classes/SPUUpdater.html#/c:objc(cs)SPUUpdater(im)checkForUpdates) for checking for updates
* [-[SPUUpdater canCheckForUpdates]](/documentation/api-reference/Classes/SPUUpdater.html#/c:objc(cs)SPUUpdater(py)canCheckForUpdates) for menu item validation. This property is also KVO compliant.

If you are using Sparkle 1, you will need to use these APIs:

* `+[SUUpdater sharedUpdater]` for creating and starting the updater automatically
* `-[SUUpdater checkForUpdates:]` for checking for updates
* `-[SUUpdater validateMenuItem:]` for menu item validation
