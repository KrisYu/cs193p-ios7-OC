- OC arguments不可以是optional，我们只能定义一个新的method
- 我们需要alloc 和 init 来创建一个object， 大部分时间我们倾向于用lazy init.

所谓的lazy init可以看一个例子：

```
#import "Deck.h"

@interface Deck()
@property (strong, nonatomic) NSMutableArray *cards;// of Card
@end

@implementation Deck

- (NSMutableArray *)cards
{
    if (!_cards) _cards = [[NSMutableArray alloc]init];
    return _cards;
}
...

@end
```
首先这个cards是一个 private NSMutableArray cards, 其次我们调用了它的getter,当我们调用的时候我们检查了_cards是否存在，如果它不存在的话我们就用alloc init来init它。

- OC 会有这种错误 ： array index out of bounds， 所以 swift optional 的设计可以避免我们写这种形式的if
- @"string" ->  string objects, @[@"1", @"2", @"3"] 创建一个NSArray. 理论上来说我们一般不用 @synthesize, 但是一旦我们implement getter 和 setter 之后，我们需要创建它，把这个synthesize明确的写出来。
- @[@"1", @"2", @"3"] 每次我们都是创建一个NSArray，我们也可以用一个static array，但是大部分是见都没有必要。
- + class method, - instance method
- -(instancetype) init 这个方法就是我们 alloc init用的方法，never call alloc without init. never call init more than once. 这两个是配对的好朋友。instancetype 作为 return type，也是非常自明的。
- flipCount的 setter，在Swift中，我们是用 property observer， didSet之类的

但是观察我们的 CardGameViewController，我们用的是默认的setter， setFlipCount 来更新:

```
@implementation CardGameViewController

- (void)setFlipCount:(int)flipCount{
//    _flipCount = flipCount;
    self.flipsLabel.text = [NSString stringWithFormat:@"Flips: %d", self.flipCount];
    NSLog(@"flipCount changed to %d", self.flipCount);
}

- (IBAction)touchCardButton:(UIButton *)sender {
    if ([sender.currentTitle length]) {
        UIImage *cardImage = [UIImage imageNamed:@"cardback"];
        [sender setBackgroundImage:cardImage forState:UIControlStateNormal];
        [sender setTitle:@"" forState:UIControlStateNormal];
    } else {
        UIImage *cardImage = [UIImage imageNamed:@"cardfront"];
        [sender setBackgroundImage:cardImage forState:UIControlStateNormal];
        [sender setTitle:@"A♣︎" forState:UIControlStateNormal];
    }
    self.flipCount++;
}


@end
```

这里一开始有一个问题，就是为什么我们要写一句 `_flipCount = flipCount;` ， 我们不是有在更新这个flipCount么，并且每次set它的时候我们同时也更新label，如果我把这一句注释到那么会发现这个flipCount的label永远都是0，不会更新。

查了SO 

<https://stackoverflow.com/questions/27964540/objective-c-underline-variable-and-self-variable-different>


>self.flipCount is a property. _flipCount is an instance variable, where that property value is stored by default. When you write self.flipCount, you, in essence, call a getter for this property: [self flipCount]. When you write self.flipCount++;, you call a getter, then a setter. Very roughly it can be written like so:

> `[self setFlipCount:[self flipCount] + 1];`
> 
> And your custom setter gets called. However, when you write _flipCount++, you access the instance variable directly, so your custom setter is ignored. That's why your flipLable is not updated in the first case.

意思是当我们用custom的setter和getter的时候我们就应当做这种 synthesize 的事，用 _instance name = instance.



