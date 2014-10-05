# Adding replay button to Amazing Brick Template
This guide is to walk you through the steps of fixing the share button popover for iPads running iOS8.

These steps expects some knowledge of xCode and Objective-C.  

**Requirements**
1. You must be using and have the most recent copy of xCode 6 from the developer center, these updates wont work for xCode 5.

**Always** create a copy or backup of your current source code in case something goes awry though these updates should be hopefully painless.

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