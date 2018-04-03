HW1 

We need only touch CardGameViewController.m to finish hw1.

```
#import "CardGameViewController.h"

@interface CardGameViewController ()

@property (weak, nonatomic) IBOutlet UILabel *flipsLabel;
@property (nonatomic) int flipCount;
@property (strong, nonatomic) Deck *deck;


@end

@implementation CardGameViewController

- (Deck *)deck
{
	 // lazy init
    if (!_deck) _deck = [[PlayingCardDeck alloc] init];
    return _deck;
}

- (void)setFlipCount:(int)flipCount{
    _flipCount = flipCount;
    self.flipsLabel.text = [NSString stringWithFormat:@"Flips: %d", self.flipCount];
    NSLog(@"flipCount changed to %d", self.flipCount);
}

- (IBAction)touchCardButton:(UIButton *)sender {
    // here we draw card from Deck
    Card* randomCard = [self.deck drawRandomCard];
    
    // if card exists, we allow flip and flipCount increase
    if (randomCard) {
        NSString *randomCardContents = [[NSString alloc] initWithString:randomCard.contents];
        
        if ([randomCardContents length]) {
            if ([sender.currentTitle length]) {
                UIImage *cardImage = [UIImage imageNamed:@"cardback"];
                [sender setBackgroundImage:cardImage forState:UIControlStateNormal];
                [sender setTitle:@"" forState:UIControlStateNormal];
            } else {
                UIImage *cardImage = [UIImage imageNamed:@"cardfront"];
                [sender setBackgroundImage:cardImage forState:UIControlStateNormal];
                [sender setTitle:randomCardContents forState:UIControlStateNormal];
            }
        }
        self.flipCount++;
    }

}


@end
```

然后看了lecture 3的开头，发现这个方法是有bug的，bug之处在于我们最多只能flip 52次，但是我们应该可以flip 104次，问题出在我们每次 touchCardButton 的时候我们都尝试抽一张 randomCard，但是问题在于这样我们就会额外多消耗 randomCard，因为这个 [randomCardContents length] 总是大于0，这样消耗牌的速度就是2x了。

正确的方法是这样：

```
- (IBAction)touchCardButton:(UIButton *)sender {
    
    if ([sender.currentTitle length]) {
        UIImage *cardImage = [UIImage imageNamed:@"cardback"];
        [sender setBackgroundImage:cardImage forState:UIControlStateNormal];
        [sender setTitle:@"" forState:UIControlStateNormal];
        self.flipCount++;
    } else {
        Card* randomCard = [self.deck drawRandomCard];
        if (randomCard) {
            UIImage *cardImage = [UIImage imageNamed:@"cardfront"];
            [sender setBackgroundImage:cardImage forState:UIControlStateNormal];
            [sender setTitle:randomCard.contents forState:UIControlStateNormal];
            self.flipCount++;
        }
    }
    
}

```