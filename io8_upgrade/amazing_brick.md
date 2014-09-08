# Amazing Brick iOS8 Update
The purpose of this document is to outline the steps required to upgrade your app based on the Amazing Brick Template.
This upgrade to your code base expects some knowledge of xCode and Objective-C.  

If you don't want to update your code directly, we can offer (our services)[http://alpinepipeline.com/pages/services] or you can use another objective-c programmer to perform the steps below.  **Always** create a copy or backup of your current source code in case something goes awry though these updates should be hopefully painless.


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
Each step is listed below for the code to replace.  