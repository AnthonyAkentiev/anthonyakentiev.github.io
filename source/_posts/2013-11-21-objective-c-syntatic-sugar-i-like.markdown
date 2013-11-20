---
layout: post
title: "Three Objective-C syntatic sugar features i like"
date: 2013-11-21 01:34
comments: true
categories: [objective-c, cheap-talk, c++ sucks] 
---

Let me start by saying that i really hate Objective-C and all that 'old-C-preprocessor' stuff Apple feeds us in 21st century.
But i hate C++ even more, because i use it more often )) Simple as that. 

Fortunately, there are some features i really DO like in Objective-C. Let me show you 3 of them:

## Method names are awesome
Get used to this style and you want C++ to be more 'Objective'. For example, this is the good Objective-C method name:

```objective-c
-(void)downloadInfoAboutUser:(NSString*)code
                  withTarget:(id)aTarget
                 andSelector:(SEL)aCallbackSelector;
```

or this:

```objective-c
-(void)setImageToCell:(UIImage*)image
             withCell:(UITableViewCell*)cell;
```

NOT this:

```objective-c
-(void)setImageToCell:(UIImage*)anImage
                 cell:(UITableViewCell*)aCell;
```

## Private means PRIVATE
I really like that **private** methods are **REALLY** private and are not **visible** in .h file. 
Example of .h file for class MainTabBarController goes here:

```objective-c
@interface MainTabBarController : UITabBarController<UITabBarControllerDelegate>

@end
```

It seems like this class has no public methods client can use? Yeah, that is what i want C++ to be.
If you need some properties or private methods - simply declare them in your .m file. Piece of cake.

## No hate for categories
Categories are really useful syntatic sugar. See this:

```objective-c
@interface NSString ( containsCategory )
     -(BOOL)isContainsString: (NSString*) substring;
@end

@implementation NSString ( containsCategory )

- (BOOL) isContainsString: (NSString*) substring {
     NSRange range = [self rangeOfString : substring];
     BOOL found = ( range.location != NSNotFound );
     return found;
}
@end
```

After declaring that - you don't need any inheritance or whatever... Simply use your NSString-s with new method:
```objective-c
NSString* imageName = @"blah-blah-blah";
BOOL isPng = [imageName isContainsString:@".png"];
```

Thanks for watching. To be continued...
