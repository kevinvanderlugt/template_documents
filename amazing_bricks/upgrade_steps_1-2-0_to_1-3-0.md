# Update steps for your existing Amazing Brick Template game to v1.3.0
The purpose of this document is to outline the steps required to upgrade your app based on the Amazing Brick Template.
These steps are only necessary if you are upgrading your existing game based on our template.  If you have a fresh updated copy, you can safely ignore these steps as the template is updated for you already!

**What's new in 1.1.0?**

1. Updated AdMob SDK to 1.12.0 and iAd Mediation Adapter.
2. Fixed a crash for iPad running iOS8 and pressing the share button
3. Added a replay button at the end of the game.
4. Removed GameCenter button if you set kImplementGameCenter to NO
5. Fix to not have images disappear when setting kObstaclesShouldTint to NO
6. Updated Chartboost Framework to 5.0.3

These steps expects some knowledge of xCode and Objective-C. If you don't want to update your code directly, we are happy to offer [our services for a **flat $75 fee**](http://alpinepipeline.com/pages/services) or you can use another programmer to perform the steps below.   

**Requirements**

1. You must be using and have the most recent copy of xCode 6 from the developer center, these updates wont work for xCode 5.

**Always** create a copy or backup of your current source code in case something goes awry though these updates should be hopefully painless.

**If you have not modified the files below, you can simply copy and replace the file from the updated template into your application folder**

## Updating Admob SDK
1. Remove the existing Admob SDK and the Mediation Adapter folders from the third_party folder and remove from xCode.
2. Drag and drop the new Admob SDK and the Mediation Adapter folders (from our updated template) into your project.
3. Add the EventKit and EventKitUI frameworks to your build phases.
4. Add the following lines of code to the viewWillDisappear method in iAdViewController.m

  ```objective-c
    [bannerView_ removeFromSuperview];
    bannerView_.delegate = nil;
    bannerView_ = nil;
  ```
5. **optional** Remove the existing folders from your "Library Search Paths" in "Build Settings"

## Steps to fix the share button
1. In AmazingBrick-Prefix.pch, add the following method.

  ```objective-c
  #define SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(v)  ([[[UIDevice currentDevice] systemVersion] compare:v options:NSNumericSearch] != NSOrderedAscending)
  ```
2. In GameOverViewController.m
  * locate the following line of code (should be around line 112).
  ```objective-c
    UIActivityViewController *controller =
      [[UIActivityViewController alloc] initWithActivityItems:@[text, url]
                                        applicationActivities:nil];
  ```
  * and add the following lines of code directly below it 
  ```objective-c
    if(SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(@"8.0")){
        UIButton *button = (UIButton *)sender;
        controller.popoverPresentationController.sourceRect = CGRectMake(button.frame.size.width/2, 0, 0, 0);
        controller.popoverPresentationController.sourceView = sender;
    }
  ```

## Added a replay button at the end of the game.
Please refer to our documentation previously written for adding the replay button.
https://github.com/kevinvanderlugt/template_documents/blob/master/amazing_bricks/adding_replay_button.md

## Removed GameCenter button if you set kImplementGameCenter to NO
1. In MenuViewController.m
  * replace the following line (around line 55)
    ```objective-c
    [[GameCenterManager sharedManager] authenticateLocalPlayer];
    ```

  * with the following 
    ```objective-c
    if(kImplementGameCenter)
    {
        [[GameCenterManager sharedManager] authenticateLocalPlayer];
    }
    self.leaderboardButton.hidden = !kImplementGameCenter;
    ```

## Fix to not have images disappear when setting kObstaclesShouldTint to NO
1. In ObstacleNode.m 
  * move the following line of code to within the `if(kObstaclesShouldTint) { }`
    ```objective-c
    node.colorBlendFactor = 1.0;
    ```

  * It should look like this
    ```objective-c
    if(kObstaclesShouldTint)
    {
        node.colorBlendFactor = 1.0;
        node.color = [self tintColorForScore:score];
    }
    ```

## Updated Chartboost Framework to 5.0.3
Simply copy and paste the new Chartboost.Framework from our updated template to your project in the /third_party/ directory

