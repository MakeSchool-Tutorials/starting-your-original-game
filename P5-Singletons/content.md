---
title: "Singletons"
slug: singletons
---     

or

#How to access an object here, there, over there, and far, far away
At a certain point you will need to share data between various objects and layers. Though you can always pass data as parameters, this can sometimes be troublesome and convoluted.

A simple design pattern that simplifies how data is shared within your game is the singleton. A singleton class is a class that only has one instance: a shared instance that is publicly accessible. The singleton will have various properties (@property and @synthesize), which will allow it to store data, and will allow any objects to access that data. For example, one CCLayer can change a number in a singleton, and then another CCLayer can read that number.

You've already seen examples of singletons:

```
CCDirector *director = [CCDirector sharedDirector];
KKInput *input = [KKInput sharedInput];
```

The sharedDirector and shareInput methods are returning static instances

- singletons

First, create your own class - give it an appropriate name. We'll use "GameData" for this example. The header file will look like:

```
#import <Foundation/Foundation.h>

//an NSObject is the most basic Objective-C object
//We subclass it since our singleton is nothing more than a wrapper around several properties
@interface GameData : NSObject

//the keyword "nonatomic" is a property declaration
//nonatomic properties have better performance than atomic properties (so use them!)
@property (nonatomic) NSMutableArray* arrayOfDataToBeStored;
@property (nonatomic) int missionCriticalNumber;
@property (nonatomic) bool somethingToBeToggled;

//Static (class) method:
+(GameData*) sharedData;
@end
```

The implementation file will look like:

```
#import "GameData.h"

@implementation GameData

@synthesize arrayOfDataToBeStored;
@synthesize missionCriticalNumber;
@synthesize somethingToBeToggled;

//static variable - this stores our singleton instance
static GameData *sharedData = nil;

+(GameData*) sharedData
{
    //If our singleton instance has not been created (first time it is being accessed)
    if(sharedData == nil)
    {
        //create our singleton instance
        sharedData = [[GameData alloc] init];

        //collections (Sets, Dictionaries, Arrays) must be initialized
        //Note: our class does not contain properties, only the instance does
        //self.arrayOfDataToBeStored is invalid
        sharedData.arrayOfDataToBeStored = [[NSMutableArray alloc] init];
    }

     //if the singleton instance is already created, return it
    return sharedData;
}

@end
```

Whenever the singleton needs to be accessed or stored, call the class method. Do not alloc it yourself:

`GameData* data = [GameData sharedData];`

To access the singleton's data, use as any other object with properties:

```
//One layer:
data.missionCriticalNumber = 999;

//Another layer:
int numberWeDesperatelyNeed = [GameData sharedData].missionCriticalNumber;
```

Hopefully this saves you from several headaches.
