- Swift print array很简单，用print， OC也没有很难，用NSLog(@"%@", myArray)
- 另一种方法来看代码，用这个 componentsJoinedByString 这个方法

```
NSString *description = @"";
    
if ([self.game.lastChosenCards count]) {
    NSMutableArray *cardContents = [NSMutableArray array];
    for (Card *card in self.game.lastChosenCards) {
        [cardContents addObject:card.contents];
    }
    
    description = [cardContents componentsJoinedByString:@" "];
}
```


- OC中没有computed property的概念，但是getter可以当我们的computed property来用,不过注意getter如果我们多次调用，里面的操作将会多次被计算

```
- (NSMutableArray *)thisRoundChoosenCards
{
    NSMutableArray *thisRoundChoosenCards = [[NSMutableArray alloc] init];
    for (Card *card in self.cards) {
        if (card.isChosen) {
            [thisRoundChoosenCards addObject:card];
        }
    }
    return thisRoundChoosenCards;
}
```

如果不看头文件或者声明的话，我们很难说这个是property还是函数，不过这个是函数 `- (NSMutableArray *)thisRoundChoosenCards;`,因为如果我们把它做getter来的话，有bug之处在于每调用一次，我们就会把这个选中的卡片添加一次，因为我发现同一个东西被添加了三次，而且这里就会觉得Swift的好，直接用 cards.filter 就可以。

