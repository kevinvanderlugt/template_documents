# Adding replay button to Amazing Brick Template
The purpose of this document is to outline the steps required to upgrade your app based on the Gestures! Template.
These steps are only necessary if you are upgrading your existing game based on our template.  If you have a fresh updated copy, you can safely ignore these steps as the template is updated for you already!

**What's new in 1.1.0?**

1. Updated AdMob SDK to 1.12.0 and iAd Mediation Adapter.
2. Fixed a crash for iPad running iOS8 and pressing the share button
3. Added a background looping soundtrack for the game.

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

## Added a background looping soundtrack for the game.
1. Add the background_track.m4a to your project from our updated template.  Be sure to add the new sound to the "Copy Bundle Resources" in "Build Phases" or you will get a crash.
2. Due to the number of changes required, you should copy and paste the following files from our updated template to your project structure.  If you have edited these files, [let me know](http://alpinepipeline.com/pages/services) and we can work together to merge the changes into your project.
  * AppDelegate.h
  * AppDelegate.m
  * MenuViewController.m
