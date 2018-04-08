- UITextView 有一个textStorage属性，这是NSMutableAttributedString的subclass，我们改变它UITextView就会跟着改变，例如我们要添加属性：

```
[self.body.textStorage addAttributes:@{ NSStrokeWidthAttributeName: @-3,
                                        NSStrokeColorAttributeName: [UIColor blackColor]
                                        } range:self.body.selectedRange];
```
这是选中的range，如果我们想要所有的都符合那么range修改为：

```
- (IBAction)changeBodySelectionColorToMatchBackgroundOfButton:(UIButton *)sender
{
    
    [self.body.textStorage addAttribute:NSForegroundColorAttributeName value:sender.backgroundColor range: NSMakeRange(0,[self.body.text length])
     ];
}
```

或者针对所有的font，我们可以通过 `@property (nonatomic, strong) UIFont *font;`来设置。比如

```
self.body.font = [UIFont preferredFontForTextStyle:UIFontTextStyleBody];
```


- View Controller LifeCycle: Apple 是鼓励 Storyboard的，这也是现在根本没有empty project这个template，当然如果我们是用 .xib 来创建view的话，可能我们就可以用到：

`- (instancetype) initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil`

但鉴于我们始于 Storyboard，基本上outlet设置好之后我们就到达 viewDidLoad，此时 geometry（bounds）还未产生，等到 viewWillAppear 这个时候，geometry 已经形成，这里面我们可以放一些需要在view中但是又比较贵的操作，比如之前看的例子，加载网络图片，和监听Notification， viewWillDisappear 中取消监听。 geometry 相关的我们放在 - (void)view{Will,Did}LayoutSubviews 中。

> awakeFromNib : This method is sent to all objects that come out of a storyboard (including your Controller).Happens before outlets are set! (i.e. before the MVC is “loaded”).Put code somewhere else if at all possible (e.g. viewDidLoad or viewWillAppear:).Anything that would go in your Controller’s init method would have to go in awakeFromNib too.

我见过 awakeFromNib 在实际项目中使用过两次。 viewDidLoad 基本都会用， viewWillLayoutSubviews有时候比如屏幕会多次变化会被使用。

- NSNotification: 以下开始听广播

```
- (void)addObserver:(id)observer // you (the object to get notified)
			 selector:(SEL)methodToInvokeIfSomethingHappens
			     name:(NSString *)name // name of station (a constant somewhere)
			   object:(id)sender; // whose changes you’re interested in (nil is anyone’s)
```

广播📢中可以有相应的消息：

```
- (void)methodToInvokeIfSomethingHappens:(NSNotification *)notification{	notification.name // the name passed above
	notification.object // the object sending you the notification
	notification.userInfo // notification-specific information about what happened}```
不需要广播的时候我们需要取消它，直到iOS 11我们都需要这么做，否则程序可能会古怪。因为NSNotification 会保留一个 unsafe retained pointer 指向程序，而如果它对一个nil发送广播，可能就会引起崩溃

```
[center removeObserver:self];[center removeObserver:self name:UIContentSizeCategoryDidChangeNotification object:nil];
```

一般在 viewWillDisappear 中我们remove 或者 dealloc, 响应系统字体变化需要监听的 Notification 是 UIContentSizeCategoryDidChangeNotification.

SO 上有一个更全面的问答关于响应系统字体: [How to detect Dynamic Font size changes from iOS Settings?](https://stackoverflow.com/questions/18951332/how-to-detect-dynamic-font-size-changes-from-ios-settings/45536515#45536515)
