# Amazing Brick iOS8 Update
The purpose of this document is to outline the steps required to upgrade your app based on the Amazing Brick Template.
This upgrade to your code base expects some knowledge of xCode and Objective-C.  

If you don't want to update your code directly, we are happy to offer [our services for a **flat $75 fee**](http://alpinepipeline.com/pages/services) or you can use another programmer to perform the steps below.  

**Always** create a copy or backup of your current source code in case something goes awry though these updates should be hopefully painless.

If you have troubles viewing or copying code from this document, the code can also be viewed on our [Github Repository](https://github.com/kevinvanderlugt/template_documents/blob/master/io8_upgrade/amazing_brick.md)

### Fixing NSLog warnings during game transition
This is a pretty small code change and can be ignored.  It is a very easy fix though so I would recommend it.

1. If you haven't changed anything in GameScene.m, you can simply copy the new GameScene.m from the template to your project.
2. If you have changed GameScene.m
  * locate the following lines of code
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

  * replace those lines with the following
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

### Upgrading Chartboost SDK
The new version of Chartboost has taken a big change.  There will be code that needs to be updated throughout the code base.
Each step below will need to be performed.

1. Remove the existing Chartboost folder from the third_party folder and remove from xCode
2. Drag and drop the new Chartboost Framework (from our updated template) into your project.
3. Make the following changes to your code packages.
  * In GameOverViewController.m make the following changes
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
  * In MenuViewController.m make the following changes
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

  * In AppDelegate.m make the following changes
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

### Upgrading Flurry SDK
This upgrade is **optional** as it provides no value yet for analytic only users.
However; it is recommended by Flurry to stay up to date, so it has been added to this update.
It is also a very easy upgrade so I would go for it.  Follow the steps below to upgrade your version.

1. Remove the existing Flurry folder from third_party
2. Drag and drop the new Flurry folder (from our updated template) into your project.

### Upgrading Appirater SDK
This is unknown if there is a problem yet.   
Currently the rate link doesn't in iOS8 anymore but waiting for GM to decide what to do

