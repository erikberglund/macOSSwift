# macOSSwift: Swizzling

### Index

* [Exchange Implementations](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_Swizzling.md#exchange-implementations)

## Exchange Implementations

This extension will make it easy to exchange implementations for two methods.

```swift
extension NSObject {
	
	class func exchangeImplementations(fromClass: AnyClass, fromSelector: Selector, toClass: AnyClass, toSelector: Selector) -> Bool {
		guard let fromMethod = class_getInstanceMethod(fromClass, fromSelector) else { return false }
		guard let toMethod = class_getInstanceMethod(toClass, toSelector) else { return false }
		method_exchangeImplementations(fromMethod, toMethod)
		return true
	}
	
	func exchangeImplementations(fromSelector: Selector, toClass: AnyClass, toSelector: Selector) -> Bool {
		guard let selfClass = object_getClass(self) else { return false }
		return NSObject.exchangeImplementations(fromClass: selfClass, fromSelector: fromSelector, toClass: toClass, toSelector: toSelector)
	}
}
```

Example Usage:

```swift

```
