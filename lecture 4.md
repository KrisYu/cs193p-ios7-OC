- 对于一个不了解Swift的人来说，ta一看Swift文件可能很多情况下会疑惑，为什么有这么多问号'?',我想，对一个OC新手来说，会觉得为什么有这么多'@',@无处不在
- id 看到OC经常有一个概念： Dynamic Binding，这个重要的概念其中有id来实现 “Figuring out the code to execute when a message is sent at runtime is called “dynamic binding.”
- selector 概念出现， @selector 直到Swift也依旧存在， selector就是一个 method `[button addTarget:self action:@selector(digitPressed:) ...];`
- 所谓introspection其实就是几个概念，我们用来检测一个object是否某个类的，给出的例子：

因为NSArray firstObject method给的实际上是id,我们之前是默默的cast了。

```
id card = [otherCards firstObject];
if ([card isKindOfClass:[PlayingCard class]]) {
    PlayingCard *otherCard = (PlayingCard *)card;
...
```

这个introspection还可以在array中 for in 使用

- `- (NSString *)description` 我们可以自己implement，来更好的debug，比如: `NSLog(@“array contents are %@”, myArray);` Swift也有这个 description，需要遵循协议CustomStringConvertible
- `[array makeObjectsPerformSelector:shootSelector]; // cool, huh?` 这个感觉是 Swift 中 map 可以做到的。
- NSNumber是primitive type的壳，我们可以直接@3或者@([create a primitive type instance]),我们之所以需要这么一个壳是因为当我们要把它们放入NSArray时候需要做这些事。
- NSDictionary immutable，要成为一个key要遵循NSCopying,

```
NSDictionary *colors = @{ @“green” : [UIColor greenColor],                          @“blue” : [UIColor blueColor],                          @“red” : [UIColor redColor] };
```
- Property List 包含如下： `NSArray, NSDictionary, NSNumber, NSString, NSDate, NSData `
- NSRange是一个struct
- UIFont必备方法 `UIFont *font = [UIFont preferredFontForTextStyle:UIFontTextStyleBody];`
- NSAttributedString 一种自带attribute的string，并不继承与NSString
