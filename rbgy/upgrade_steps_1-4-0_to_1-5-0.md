# Update steps for your existing RBGY game from v1.4.0 to v1.5.0
The purpose of this document is to outline the steps required to upgrade your app based on the RBGY Template.
These steps are only necessary if you are upgrading your existing game based on our template.  If you have a fresh updated copy, you can safely ignore these steps as the template is updated for you already!

**What's new in 1.5.0?**

1. Fixed a crash for iPad running iOS8 and pressing the share button
3. Updated Chartboost Framework to 5.0.3

These steps expects some knowledge of xCode and Objective-C. If you don't want to update your code directly, we are happy to offer [our services for a **flat $75 fee**](http://alpinepipeline.com/pages/services) or you can use another programmer to perform the steps below.   

**Requirements**

1. You must be using and have the most recent copy of xCode 6 from the developer center, these updates wont work for xCode 5.

**Always** create a copy or backup of your current source code in case something goes awry though these updates should be hopefully painless.

**If you have not modified the files below, you can simply copy and replace the file from the updated template into your application folder**

## Steps to fix the share button
1. In rbgy-Prefix.pch, add the following method.

  ```objective-c
  #define SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(v)  ([[[UIDevice currentDevice] systemVersion] compare:v options:NSNumericSearch] != NSOrderedAscending)
  ```
2. In GameViewController.m
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

## Updated Chartboost Framework to 5.0.3
Simply copy and paste the new Chartboost.Framework from our updated template to your project in the /third_party/ directory

