# GCHelper

GCHelper is a Swift implementation for GameKit built off of the GameKitHelper class described in [this tutorial](http://www.raywenderlich.com/60980/game-center-tutorial-how-to-make-a-simple-multiplayer-game-with-sprite-kit-part-1) by [Ali Hafizji](https://twitter.com/Ali_hafizji). If you like this project, feel free to star it or watch it for updates. If you end up using it in one of your apps, I would love to hear about it! Let me know on Twitter [@jackcook36](https://twitter.com/jackcook36).

**Disclaimer:** I am by no means a Swift expert. Or a Markdown expert. If you see any issues in my code (or the README), please correct it. It will help me learn more than I already know and probably help out whoever is using the project as well. Thanks in advance.

## Features

- Authenticate the user in one line of code
- Update achievements, also in one line of code
- Create a match in...
- Just about everything is do-able with just one line of code

---
## Implementation

### Authenticating the User
Before doing anything with Game Center, the user needs to be signed in. This instance is often configured in your app's `application:didFinishLaunchingWithOptions:` method

```swift
func application(application: UIApplication!, didFinishLaunchingWithOptions launchOptions: NSDictionary!) -> Bool {
    GCHelper.sharedInstance.authenticateLocalUser()
}
```

### Creating a Match
A match needs to be created in order for a multiplayer game to work.

```swift
GCHelper.sharedInstance.findMatchWithMinPlayers(2, maxPlayers: 4, viewController: self, delegate: self)
```

### Sending Data
Once a match has been created, you can send data between players with `NSData` objects.

```swift
var success = GCHelper.sharedInstance.match.sendDataToAllPlayers(data, withDataMode: GKMatchSendDataMode.Reliable, error: nil)
if (!success) {
    NSLog("An unknown error occured while sending data")
}
```
> I realize that this method isn't actually provided by GCHelper, but it's definitely worth noting in here.

### Report Achievement Progress
If you have created any achievements in iTunes Connect, you can access those achievements and update their progress with this method. The `percent` value can be set to zero or 100 if percentages aren't used for this particular achievement.

```swift
GKHelper.sharedInstance.reportAchievementIdentifier("achievementIdentifierGoesHere", percent: 100.0)
```

### Show GKGameCenterViewController
GCHelper also contains a method to display Apple's GameKit interfaces. These can be used to show achievements, leaderboards, or challenges, as is defined by the use of the `viewState` value. In this case, `self` is the presenting view controller.

```swift
GCHelper.sharedInstance.showGameCenter(self, viewState: GKGameCenterViewControllerState.Achievements)
```
---
## GCHelperDelegate Methods
To receive updates about the match, there are three delegate methods that you can implement in your code.

### Beginning of Match
This method is fired when the match begins.

```swift
func matchStarted() {
}
```

### End of Match
This method is fired at the end of the match, whether it is due to an error such as a player being disconnected or you actually ending the match.

```swift
func matchEnded() {
}
```

### Receiving Data
I mentioned how to send data earlier, this is the method you would use to receive that data.

```swift
func match(match: GKMatch, didReceiveData data: NSData, fromPlayer playerID: String) {
}
```

---
## License

GCHelper is available under the MIT license. See the LICENSE file for details.
