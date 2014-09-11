# Swipe the Arrows iOS8 Update
The purpose of this document is to outline the steps required to upgrade your app based on the Swipe the Arrows Template.
This upgrade to your code base expects some knowledge of xCode and Objective-C.  

**Requirements**

1. You must be using and have the most recent copy of xCode 6 from the developer center, these updates wont work for xCode 5.
2. Apple is currently approving apps for iOS8 but won't release until Sept 17th, 2014.  By doing this update, your app wont be able to release until then.
3. These steps are only necessary if you are upgrading your existing game based on our template.  If you have a fresh updated copy, you can safely ignore these steps as the template is updated for you already!

If you don't want to update your code directly, we are happy to offer [our services for a **flat $75 fee**](http://alpinepipeline.com/pages/services) or you can use another programmer to perform the steps below.  

**Always** create a copy or backup of your current source code in case something goes awry though these updates should be hopefully painless.

### Upgrading Appirater SDK
The URL structure of the link for sending users to review your app has changed in iOS8.
This change is required or when a user clicks the review link, they won't be taken to your app page.

1. Copy and Replace the entire Appirater folder in /third_party/ from our template to your app folder.

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

### Fixing the How to Play hand position
In a new build targeting iOS 8.0+ the hands in the HowToPlayViewController will not be properly positioned.
In order to fix this issue we have made the update as simple as we could.  

1. If you have not changed HowToPlayViewController.m, simply copy and replace the file from the updated template
2. If you have changed HowToPlayViewController.m
  * paste the following lines of code before the @end at the end of this file
  ```objective-c
  -(void)viewDidLoad
  {
      [self positionHands];
  }

  -(void)positionHands
  {
      self.slideRightHand.translatesAutoresizingMaskIntoConstraints = YES;
      CGRect leftHandFrame = self.slideRightHand.frame;
      leftHandFrame.origin.x = self.view.frame.size.width * 0.15;
      self.slideRightHand.frame = leftHandFrame;
      
      self.slideLeftHand.translatesAutoresizingMaskIntoConstraints = YES;
      CGRect rightHandFrame = self.slideLeftHand.frame;
      rightHandFrame.origin.x = self.view.frame.size.width * 0.70;
      self.slideLeftHand.frame = rightHandFrame;
  }
  ```

### Upgrading Flurry SDK
This upgrade is **optional** as it provides no value yet for analytic only users.
However; it is recommended by Flurry to stay up to date, so it has been added to this update.
It is also a very easy upgrade so I would go for it.  Follow the steps below to upgrade your version.

1. Remove the existing Flurry folder from third_party
2. Drag and drop the new Flurry folder (from our updated template) into your project.
