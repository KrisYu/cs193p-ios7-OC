- `@property (nonatomic, strong) NSMutableArray *cards; // of Card` 之所以强调 of Card 是因为 OC 不同于 Swift，Swift 是强类型，我们可以定义类型是 [Cards]，然而 OC 就只可能是 NSMutableArray，我们甚至可以往里面任意放东西。
- Add nil to an NSMutableArray会导致软件崩溃
- 可以注意一下OC的命名方法 `- (Card *)cardAtIndex:(NSUInteger) index;`,如果是Swift，那么命名就是`func card(at index:Int) -> Card`
- 被一个bug困了很久，后来发现是getter写错了

```
- (CardMatchingGame *)game
{
    if (!_game) _game = [[CardMatchingGame alloc] initWithCardCount:[self.cardButtons count]
                                                         usingDeck: [self createDeck]];
    return _game;
}
```
注意这里是我们检查 _game 是否存在，如果不存在我们就init 它，否则我们就直接返回。

- 还不习惯OC的地方，比如String，我们需要这样来init它 `    self.scoreLabel.text = [NSString stringWithFormat:@"Score: %d",self.game.score];`， 而Swift我们则可以 `self.scoreLabel.text = 
\(self.game.score)`,这个快速转为String

- 还有debug，Swift的print可以print任何东西，而NSLog我们尝试log一个nil就会崩溃。

- 一个尝试debug 'unrecognized selector sent to instance' error 的方法

> You can easily track such crashes using Exception Breakpoints.
> Open the Breakpoint Navigator and add the Exception Breakpoints. 
> Exception choose Objective-C.

图文版： <https://stackoverflow.com/a/36590394/3608824>

