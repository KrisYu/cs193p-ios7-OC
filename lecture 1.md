- Objective-C 可以发送messgae 给nil，意思是我们可以直接在nil上调用方法，这样也不会崩溃，这大概也是Swift设计optional的重要原因吧。

- OC中的object都是住在heap的，heap是要allocate memory和free memory的地方，意思是不同于C语言，C语言中可以住在stack上 `double myList[3]`,但是对于OC，我们都是住在heap里的，意思是object pointer将会无处不在。

- lucky thing，我们不用管理内存，OC和Swift都用的是ARC，automatic reference counting，所以我们就不用去free memory，因为它会被OC/Swift会帮我们管理。

- 所以一个property会有 strong 和 weak ，weak的意思是仅当有strong pointer指向它的时候我们让它存在于内存中，一旦没有strong pointer指向它，那么它就不存在于内存中，同时这个pointer自动指向nil。nil就是pointer不指向任何东西。（ARC可能导致retain memory cycle，所以我们才会有strong/weak出现，这个问题OC/Swift都需要解决，默认Swift的property是strong）

-  nonatomic 不是thread-safe，我们最多只能有一个thread在使用它。如果没有说atomic，那么就会有很多locking code.

- property会自动有getter 和 setter 方法，虽然我们没有看到，但是它们存在，getter就是property名字，setter是 set + property name。不过我们调用的话还是 instance variable.property name，如果把它放在等号左边就相当于setter，否则我们默认就是getter.

- BOOL 因为是primitive type，并没有存在于heap上，所以我们不给他添加strong/weak. BOOL property 的getter是 is + property type.

- 比如看这样一个类

```
// Card.h
# import <Foundation/Foundation.h>

@interface Card: NSObject
@property (strong, nonatomic) NSString *contents;

@end
``` 
实际上在 Card.m 中是这样的：

```
// Card.m
#import "Card.h"

@interface Card()
// private property could be write here
@end

@implementation Card

// from here, the setter and getter are generated automaticlly
// you cannot see them, but they're exist.
@synthesize contents = _contents;

// getter
- (NSString *)contents 
{
   return _contents;
}

// setter
- (void)setContents: (NSString *)contents 
{
  _contents = contents;
}

@end
```

   ​