# Adding replay button to Amazing Brick Template
This guide is to walk you through the steps of adding a replay button to game over screen of our Amazing Brick template.

These steps expects some knowledge of xCode and Objective-C.  

**Requirements**
1. You must be using and have the most recent copy of xCode 6 from the developer center, these updates wont work for xCode 5.

If you don't want to update your code directly, we are happy to offer [our services for a **flat $75 fee**](http://alpinepipeline.com/pages/services) or you can use another programmer to perform the steps below.  

**Always** create a copy or backup of your current source code in case something goes awry though these updates should be hopefully painless.

## Steps to add the replay button unwind segue

1. In GameScene.m, add the following method.
```objective-c
-(void)setupReplay
{
  [self.children enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
      SKNode* child = obj;
      [child removeAllActions];
  }];
  
  [self removeAllChildren];
  
  _score = 0;
  _maxPlayerY = 0;
  _highestObstacle = 0;
  self.highObstacle = nil;
  
  [self setupScene];
  // When the game is initialized
  [self addObstacle:NO];
  [self addObstacle:YES];
  [self addObstacle:YES];
  [self addObstacle:YES];
}
```

Add the following method header in GameScene.h
```objective-c
-(void)setupReplay;
```

2.  Add the following method to GameViewController.m before the @end
```objective-c
- (IBAction)replay:(UIStoryboardSegue *)replaySegue
{
    SKView * skView = (SKView *)self.view;
    GameScene *scene = (GameScene *)skView.scene;
    [scene setupReplay];
}
```

3. Add your button in the StoryBoard and setup your autolayout constraints. Since these are so tricky to teach in a document, I recommend learning from [Apple's documentation](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/Introduction/Introduction.html)

4. Control drag from the button to the orange exit box in the top of the view controller.  Select the replay: method.   !(https://github.com/kevinvanderlugt/template_documents/blob/e564e41dabebdc5d17b84fb1f5d55db2fecdd8c3/amazing_bricks/replay_button_action.png)

You should now have a working replay button.  Be sure to test it out and make sure its working with your theme!
Thanks for reading.
**Cheers!**
