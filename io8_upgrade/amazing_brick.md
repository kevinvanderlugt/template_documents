# Amazing Brick iOS8 Update
The purpose of this document is to outline the steps required to upgrade your app based on the Amazing Brick Template.
To perform this upgrade, some knowledge of xCode and Objective-C is expected.  

**Requirements**

1. You must be using the most recent copy of xCode 6 from the developer center; these updates wont work for xCode 5.
2. Apple is currently approving apps for iOS8 but won't release them until Sept 17th, 2014.  If you update now, your app wont be able to release until then.

If you don't want to update your code yourself, we are happy to offer [our services for a **flat $75 fee**](http://alpinepipeline.com/pages/services).  

**Always** create a copy or backup of your current source code in case something goes awry, though these updates should hopefully be painless.

If you have trouble viewing or copying code from this document, the code can also be viewed on our [Github Repository](https://github.com/kevinvanderlugt/template_documents/blob/master/io8_upgrade/amazing_brick.md)

### Upgrading Appirater SDK
The URL structure of the link for sending users to review your app has changed in iOS8.
This change is required!  Otherwise, when a user clicks the review link, they won't be taken to your app page.

1. Copy and Replace the entire Appirater folder in /third_party/ from our template to your app folder.

### Upgrading Chartboost SDK
The new version of Chartboost has has a big change.  There will be code that needs to be updated throughout the code base.
Each step below will need to be performed.

1. Remove the existing Chartboost folder from the third_party folder, and remove it from xCode.
2. Drag and drop the new Chartboost Framework (from our updated template) into your project.
3. Make the following changes to your code packages:
  * In GameOverViewController.m make the following changes:
    1. Replace the following 
    ```objective-c
    #import "Chartboost.h"
    ``` 
    With 
    ```objective-c
    #import <Chartboost/Chartboost.h>
    ```
    2. Replace the following 
    ```objective-c
    [[Chartboost sharedChartboost] showInterstitial:CBLocationGameOver];
    ``` 
    With 
    ```objective-c
    [[Chartboost sharedChartboost] showInterstitial:CBLocationGameOver];
    ```
  * In MenuViewController.m make the following changes:
    1. Replace the following 
    ```objective-c
    #import "Chartboost.h"
    ``` 
    With 
    ```objective-c
    #import <Chartboost/Chartboost.h>
    ```
    2. Replace the following 
    ```objective-c
    [[Chartboost sharedChartboost] showMoreApps:CBLocationMainMenu];
    ``` 
    With 
    ```objective-c
    [Chartboost showMoreApps:CBLocationMainMenu];
    ```

  * In AppDelegate.m make the following changes:
    1. Replace the following 
    ```objective-c
    #import "Chartboost.h"
    ```
    With 
    ```objective-c
    #import <Chartboost/Chartboost.h>
    ```
    2. Replace the following 
    ```objective-c
    [[Chartboost sharedChartboost] cacheMoreApps:CBLocationMainMenu];
    ``` 
    With 
    ```objective-c
    [Chartboost cacheMoreApps:CBLocationMainMenu];
    ```

### Fixing iAP issue if not configured correctly
This fix is optional but recommended just for easier reskinnning.  If you happened to set the `#define kIAPEnabled YES` but did not setup your iAP product ID, the app would throw an error.  With this update, we added logging for this error.

1. Simply copy and replace InAppPurchaseManager.m and InAppPurchaseManager.h from our updated template to your project directory.

### Fixing NSLog warnings during game transition
This is a pretty small code change and can be ignored.  It is a very easy fix though so I would recommend it.

1. If you haven't changed anything in GameScene.m, you can simply copy the new GameScene.m from the template to your project.
2. If you have changed GameScene.m
  * locate the following lines of code:
  ```objective-c
  [self enumerateChildNodesWithName:@"obstacle"
                        usingBlock:^(SKNode *node, BOOL *stop) {
                               
                           [node runAction:shakeRepeat
                                completion:^{
                                    if(shouldEnd)
                                    {
                                        NSAssert(self.endGameCallback, @"Forgot to set endGameCallBack");
                                        self.endGameCallback();
                                    }
                                }];
                       }];
  ```

  * replace those lines with the following:
  ```objective-c
  [self enumerateChildNodesWithName:@"obstacle"
                         usingBlock:^(SKNode *node, BOOL *stop) {
                             [node runAction:shakeRepeat];
                         }];
  if(shouldEnd)
  {
      SKAction *wait = [SKAction waitForDuration:shakeRepeat.duration];
      SKAction *end = [SKAction runBlock:^{
          NSAssert(self.endGameCallback, @"Forgot to set endGameCallBack");
          self.endGameCallback();
      }];
      SKAction *waitAndEnd = [SKAction sequence:@[wait, end]];
      [self runAction:waitAndEnd];
  }
  ```



### Upgrading Flurry SDK
This upgrade is **optional** as it provides no value yet for analytic only users.
However; it is recommended by Flurry to stay up to date, so it has been added to this update.
It is also a very easy upgrade so I would go for it.  Follow the steps below to upgrade your version.

1. Remove the existing Flurry folder from third_party
2. Drag and drop the new Flurry folder (from our updated template) into your project.
