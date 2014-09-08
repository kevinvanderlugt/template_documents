# Amazing Brick iOS8 Update
The purpose of this document is to outline the steps required to upgrade your app based on the Amazing Brick Template.



```objective-c
#import <Chartboost/Chartboost.h>

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  // initialize the Chartboost library
  [Chartboost startWithAppId:@"YOUR_CHARTBOOST_APP_ID" 
                appSignature:@"YOUR_CHARTBOOST_APP_SIGNATURE" 
                    delegate:self];
   
  // Show an interstitial ad
  [Chartboost showInterstitial:CBLocationHomeScreen];
}
```