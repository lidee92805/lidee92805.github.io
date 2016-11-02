---
layout: post
title: "Objective-C id as Swfit Any"
date: 2016-11-02
tags: [iOS]
comments: true

---

# Objective-C id as Swfit Any

[翻译自苹果开发博客Objective-C id as Swift Any](https://developer.apple.com/swift/blog/?id=39)

翻译的不好勿喷

* Objective-C id as Swift Any

  ​相比之前版本Swift 3与Objective-C API连接的更加强大了。比如，Swift 2把Objective-C中的**id**类型映射为Swift中的**AnyObject**类型。Swfit 2也提供了**String**，**Array**，**Dictionary**，**Set**和一些数字等桥接值类型隐式转换成**AnyObject**，以便Swift原生类型方便的使用**NSString**，**NSArray**或者其他Foundation容器类型的Cocoa API。这些转换显得自相矛盾，使用**AnyObject**理解起来更困难最终导致BUG。

  ​在Swift 3中Objective-C中的**id**类型映射为Swift中的可以表示为任何类型的**Any**类型，不管是class，enum，struct或者任何Swift类型。由于Swift定义的值类型能传递给Objective-C API并且取出，省去了手动封装，使得在Swift中使用Objective-C API更加灵活。同时也扩展到了Objective-C的集合类型**NSArray**,**NSDictionary**,**NSSet**这些之前只能接受**AnyObject**类型现在能接受**Any**类型。对于可哈西的容器，比如**Dictionary**和**Set**，新类型**AnyHashable**可以持有任何遵从**Hashable**协议的类型。以下是从Swift 2到Swift 3类型映射的变化：

  ​

| Objective-C    | Swift 2               | Swift 3            |
| -------------- | --------------------- | ------------------ |
| id             | AnyObject             | Any                |
| NSArray *      | [AnyObject]           | [Any]              |
| NSDictionary * | [NSObject: AnyObject] | [AnyHashable: Any] |
| NSSet *        | Set<NSObject>         | Set<AnyHashable>   |

​	

​	大多数情况你不用特意去改动你的代码。代码在Swift 2中值类型隐式转换成**AnyObject**在Swift 3中将会转换成**Any**。然而为了获得Swfit 3最佳的体验，这里展示了你将要修改的类型中的属性和方法。如果你的代码中显式的使用了**AnyObject**或者Cocoa类比如**NSString**，**NSArray**或者**NSDictionary**，你需要使用**as NSString**或者**as String**进行显式转换，因为在Swift 3中隐式转换已经不被允许了。从Swift 2迁移到Swift 3，Xcode会自动做小幅度修改以保证代码能被编译，但这不够优雅。这篇文章将说明一些你需要修改和使用**Any**时要注意的陷阱。

*   Overriding methods and conforming to protocols

    当子类一个Objective-C类，覆写方法或者遵从一个Objective-C协议，父类方法中使用id类型时必须更新方法签名。比如**NSObject**类的**isEqual:**方法和**NSCopying**协议的**copyWithZone:**方法。创建一个遵从**NSCopying**协议的**NSObject**子类，在Swift 2中你要这样写：

    ```swift
    //Swift 2
    class Foo: NSObject, NSCopying {
      override func isEqual(_ x: AnyObject?) -> Bool { ... }
      func copyWithZone(_ zone: NSZone?) -> AnyObject { ... }
    }
    ```

    在Swift 3中，把方法名字从**copyWithZone(_:)**改成**copy(with:)**，还需要把方法签名中的**AnyObject**用**Any**代替：

    ```swift
    //Swift 3
    class Foo: NSObject, NSCopying {
      override func isEqual(_ x: Any?) -> Bool { ... }
      func copy(with zone: NSZone?) -> Any { ... }
    }
    ```

*   Untyped Collections

        Plist，JSON和NSUserDefault在Cocoa中很常见，Cocoa原生表示为非强类型集合。在Swift 2中要达到相同的目的需要构建包含**AnyObject**或者**NSObject**元素的**Array**，**Dictionary**或者**Set**，依靠隐式转换成能处理的类型：

    ```swift
        //Swift 2
        struct State {
          var name: String
          var abbreviation: String
          var population: Int
          
          var asPropertyList: [NSObject: AnyObject] {
            var result: [NSObject: AnyObject] = [:]
            //Implicit conversions turn String into NSString here...
            result["name"] = self.name
            result["abbreviation"] = self.abbreviation
            //...and Int into NSNumber here.
            result["population"] = self.population
            return result
          }
        }
        let california = State(name: "California", abbreviation: "CA", population: 39_000_000)
        NSNotification(name: "foo", object: nil, userInfo: california.asPropertyList)
    ```

        或者使用Cocoa的容器类，比如**NSDictionary**：

    ```swift
        //Swift 2
        struct State {
          var name: String
          var abbreviation: String
          var population: Int
          
          var asPropertyList: NSDictionary {
            var result = NSMutableDictionary()
            //Implicit conversions turn String into NSString here...
            result["name"] = self.name
            result["abbreviation"] = self.abbreviation
            //...and Int into NSNumber here.
            result["population"] = self.population
            return result.copy()
          }
        }
        let california = State(name: "California", abbreviation: "CA", population: 39_000_000)
        //NSDictionary then implicitly converts to [NSObject: AnyObject] here.
        NSNotification(name: "foo", object: nil, userInfo:california.asPropertyList)
    ```

        在Swift 3中，隐式转换没有了，所以上面的代码片段不能工作。迁移助手建议将每个需要转换的值使用**as**进行转换使代码正常工作，但是这里有更好的解决方案。Swift现在引入了Cocoa API接受**Any**和**AnyHashable**的集合，所有我们可以修改集合类型使用**[AnyHashable: Any]**代替**[NSObject: AnyObject]**或者**NSDictionary**而不需要修改其他代码：

    ```swift
        //Swift 3
        struct State {
          var name: String
          var abbreviation: String
          var population: Int
          
          //Change the dictionary type to [AnyHashable: Any] here...
          var asPropertyList: [AnyHashable: Any] {
            var result: [AnyHashable: Any] = [:]
            //No implicit conversions necessary, since String and Int are subtypes
            //of Any and AnyHashable
            result["name"] = self.name
            result["abbreviation"] = self.abbreviation
            result["population"] = self.population
            return result
          }
        }
        let california = State(name: "California", abbreviation: "CA", population: 39_000_000)
        //...and you can still use it with Cocoa API here
        Notification(name: "foo", object: nil, userInfo: california.asPropertyList)
    ```

*   The AnyHashable Type

        Swift的**Any**类型可以是任何类型，但是**Dictionary**和**Set**的key需要可哈希化，所以**Any**太笼统。Swift 3标准库提供了一种**AnyHashable**的新类型。类似**Any**它能代表所有可哈希的类型，所以**String**、**Int**和其他可哈希类型的值能隐式作为**AnyHashable**值，并且**AnyHashable**内部的值能使用**is**、**as!**或者**as?**进行动态检查。当从Objective-C中导入非类型化的**NSDictionary**或者**NSSet**使用**AnyHashable**，在纯Swift中构建异构集合和字典也很有用。

*   Explicit Conversion for Unbridged Contexts

        在有些情况下，Swift不能自动桥接C和Objective-C结构体。例如，一些C和Cocoa API使用**id ***指针作为"输出"或者"输入输出"参数，Swift不能静态确定如何使用指针，因为它无法自动对内存中的值进行桥接转换。在这种情况下，指针显示为**UnsafePointer<AnyObject>**。如何你需要使用这些未桥接的API，你需要显示的使用**as Type**或者**as AnyObject**进行转换。

    ```objective-c
        //ObjC
        @interface Foo
          
    - (void)updateString:(NSString **)string;
    - (void)updateObject:(id *)obj;

    @end
    ```

    ​

    ```swift
    // Swift
    func interactWith(foo: Foo) -> (String, Any) {
      var string = "string" as NSString //explicit conversion
      foo.updateString(&string) //parameter imports as UnsafeMutablePointer<NSString>
      let finishedString = string as String
      
      var object = "string" as AnyObject
      foo.updateObject(&object) // parameter imports as UnsafeMutablePointer<AnyObject>
      let finishedObject = object as Any
      
      return (finishedString, finishedObject)
    }
    ```

    另外，Objective-C的协议在Swift中依然受类限制，所以你不能使Swift的结构体和枚举直接遵从Objective-C的协议或者使用轻量泛型。你要使用这些协议和API需要显示转换**String as NSString**，**Array as NSArray**。

*   AnyObject Member Lookup

        **Any**没有**AnyObject**的方法查找。这可能破坏Swift 2中查找属性或者给非类型的Objective-C对象发送消息的代码。比如：

    ```swift
        // Swift 2
        func foo(x: NSArray) {
          // Invokes -description by magic AnyObject lookup
          print(x[0].description)
        }
    ```

        在Swift 3中编译**Any**没有**description**成员。你可以使用**x[0] as AnyObject**转换重新获取动态查找：

    ```swift
        // Swift 3
        func foo(x: NSArray) {
          // Result of subscript is now Any, needs to be coerced to get method lookup
          print((x[0] as AnyObject).description)
        }
    ```

        另外强转为你期望的对象：

    ```swift
        func foo(x: NSArray) {
          // Cast to the concrete object type you expect
          print((x[0] as! NSObject).description)
        }
    ```

*   Swift Value Types in Objective-C

        **Any**能代表任何结构体、枚举、元类或者其他你能定义的Swift类型。Objective-C桥接在Swift 3中可以反过来提供任何Swift值作为id兼容的Objective-C对象。这样在Cocoa容器存储Swift类型值更加简单。例如，在Swift 2你需要把数据转换成类或者用box封装附加到**NSNotification**上：

    ```swift
        // Swift 2
        struct CreditCard { number: UInt64, expiration: NSDate }

        let PaymentMade = "PaymentMade"

        //We can't attach CreditCard directly to the notification, since it
        //isn't a class, and doesn't bridge.
        //Wrap it in a Box class.
        class Box<T> {
          let value: T
          init(value: T) { self.value = value }
        }

        let paymentNotification = NSNotification(name: PaymentMade, object: Box(value: CreditCard(number: 1234_0000_0000_0000, expiration: NSDate())))
    ```

        Swift 3，我们可以不用封装直接把对象附加到notification上：

    ```swift
        // Swift 3
        let PaymentMade = Notification.Name("PaymentMade")

        //We can associate the CreditCard value directly with the Notification
        let paymentNotification = Notification(name: PaymentMade, object: CreditCard(number: 1234_0000_0000_0000), expiration: Date()))
    ```

        在Objective-C中，如果**CreditCard**存在原始的Swift类型将表现出**id**兼容，遵从**NSObject**的对象使用Swift的**Equatable**，**Hashable**和**CustomStringConvertible**的实现方法来实现**isEqual:**，**hash**和**description**方法。在Swift值会动态转换成原始的类型：

    ```swift
        // Swift 3
        let paymentCard = paymentNotification.object as! CreditCard
        print(paymentCard.number) // 1234000000000000
    ```

        注意，在Swift 3.0，一些常见的Swift和Objective-C结构体类型将桥接为不透明对象，而不是作为惯用的Cocoa对象。例如，**Int**，**UInt**，**Double**和**Bool**桥接成**NSNumber**，其他**Int8**，**UInt16**等数值类型只桥接成不透明对象。Cocoa结构体例如**CGRect**，**CGPoint**和**CGSize**也桥接成不透明对象即使大多数使用Cocoa API共同工作的对象期望它们被封装成**NSValue**实例。如果你看见了**unrecognized selector sent to _SwiftValue**的错误，指出Objective-C代码尝试对不透明的Swift值类型调用方法，你需要手动把值封装成类的实例。

        还有是要注意Optional。**Any**能接受任何东西包括Optional，所以可能未经过检查传递给Objective-C API一个装包的Optional，即使API声明为**nonnull id**。这将通常导致运行时**_SwiftValue**错误而不是编译时的错误。Xcode8.1内置Swift 3.0.1处理数值类型，Objective-C结构体和Optional桥接**NSNumber**，**NSValue**和**Optional**时在实现的位置提供了显式的提示：

    * [SE–0139: Bridge Numeric Types to NSNumber and Cocoa Structs to NSValue](https://github.com/apple/swift-evolution/blob/master/proposals/0139-bridge-nsnumber-and-nsvalue.md)
    * [SE–0140: Warn when Optional converts to Any, and bridge Optional As Its Payload Or NSNull](https://github.com/apple/swift-evolution/blob/master/proposals/0140-bridge-optional-to-nsnull.md)

    为了避免向前兼容性问题，您不应依赖不透明对象的 _SwiftValue 类的实现细节，因为 Swift 的未来版本可能允许更多的 Swift 类型桥接为常用的Objective-C类。

*   Linux Portability

        Swift程序运行在Linux上，Swift核心库使用了用Swift编写的Foundation没有使用Objective-C的运行时。**id-as-Any**允许核心库直接使用原生Swift的**Any**和标准库的值类型，兼容苹果平台上使用Objective-C **Foundation**实现的代码。自从Swift在Linux上不再兼容Objective-C,像**string as NSString**或者**value as AnyObject**这样的转换也不再支持。Swift代码尝试只使用值类型在Cocoa和Swift核心库之间是可移植的。

*   Learning More

    * [SE-0072: Fully eliminate implicit bridging conversions from Swift](https://github.com/apple/swift-evolution/blob/master/proposals/0072-eliminate-implicit-bridging-conversions.md)
    * [SE–0116: Import Objective-C id as Swift Any type](https://github.com/apple/swift-evolution/blob/master/proposals/0116-id-as-any.md)
    * [SE–0131: Add AnyHashable to the standard library](https://github.com/apple/swift-evolution/blob/master/proposals/0131-anyhashable.md)