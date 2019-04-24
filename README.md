# sample-swift-copyUILabel

# 概要
通常のUILabelは、フィールドの長押しでコピーができません。
UITextFieldのようにフィールドの長押しでコピーができるようにする方法です。

# 実装方法
UILabelで長押しでコピーするために、以下のコードを、
CopyUILabel.swiftとして追加します。
このCopyUILabelを、storyboardで、
長押しでコピーしたいUILabelの
Custom Classで設定します。

```
class CopyUILabel: UILabel {
    override init(frame: CGRect) {
        super.init(frame: frame)
        self.copyInit()
    }

    required init?(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)
        self.copyInit()
    }

    func copyInit() {
        self.isUserInteractionEnabled = true
        self.addGestureRecognizer(UILongPressGestureRecognizer(target: self, action: #selector(CopyUILabel.showMenu(sender:))))
    }

    @objc func showMenu(sender:AnyObject?) {
        self.becomeFirstResponder()
        let contextMenu = UIMenuController.shared
        if !contextMenu.isMenuVisible {
            contextMenu.setTargetRect(self.bounds, in: self)
            contextMenu.setMenuVisible(true, animated: true)
        }
    }

    override func copy(_ sender: Any?) {
        let pasteBoard = UIPasteboard.general
        pasteBoard.string = text
        let contextMenu = UIMenuController.shared
        contextMenu.setMenuVisible(false, animated: true)
    }

    override var canBecomeFirstResponder: Bool {
        return true
    }

    override func canPerformAction(_ action: Selector, withSender sender: Any?) -> Bool {
        return action == #selector(UIResponderStandardEditActions.copy)
    }
}
```

---
# 環境
- Xcode 10.2
- Swift 5

---
# 参考文献
- Apple公式マニュアル [canPerformAction](https://developer.apple.com/documentation/uikit/uiresponder/1621105-canperformaction)
- Apple公式マニュアル [canBecomeFirstResponder] (https://developer.apple.com/documentation/uikit/uiresponder/1621130-canbecomefirstresponder)
