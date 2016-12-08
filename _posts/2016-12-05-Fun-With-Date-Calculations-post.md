---
layout: post
title: "Fun With Date Calculations"
date: 2016-12-05
tags: [iOS]
comments: true

---



[原文地址Fun With Date Calculations](http://useyourloaf.com/blog/fun-with-date-calculations/)



# 趣玩日期计算

如果粗心的话日期计算是一个常见的陷阱。怎样计算出今天、明天、5天后、5个月后的时间？1月31号加上1个月是什么日期？如果你通过添加秒数计算1小时或者1天后，你可能做错了。



## 日期计算

第一个先要解决的问题是计算出一个日期来发送通知。日期需要是距离今天可变的数量。我不关心什么时间发通知只要是在这天就行。

提醒：iOS8之后解决方案很简单只要你熟悉基本的**Date**，**DateComponents**和**Calendar**类。



## Date，DateComponents和Calendar

*如果你使用Objective-C则是NSDate，NSDateComponents和NSCalendar*

### 创建日期

在iOS中获取日期最基本的方式是实用**Date**类：

```swift
let now = Date()
//Nov 20, 2016, 6:55 PM

let later = Date(timeIntervalSinceNow: 300)
//Nov 20, 2016, 7:00 PM

let evenLater = Date(timeInterval: 300, since: later)
//Nov 20, 2016, 7:05 PM
```

**一个Date是一个固定的时间点**。它不依赖于用户的语言环境,日历或时区。如果你想做日期和时间计算不要添加一个固定的常数。例如,不能保证每天都有86400秒。

### 日期组件

其他创建date的方法是使用**DateComponents**结构体。它包含创建日期和时间需要的组件。

```swift
var jan10 = DateComponents()
jan10.hour = 9
jan10.minute = 30
jan10.day = 10
jan10.month = 1
jan10.year = 2017
jan10.timeZone = TimeZone(abbreviation: "GMT")
```

**你不需要设置所有的组件，只需要设置当你匹配或者查找日期时有用的**。提醒你可以设置任何值，没有无效日期提醒：

```swift
//2月31号？
dateComponents.day = 31
dateComponents.month = 2
```

### Calendar类

**Date**和**DateComponents**都不了解用户的日历。为用户获取当前的日历：

```swift
let calendar = Calendar.current
```

如果你使用自动更新的calendar当用户更改位置将自动更新时间：

```swift
let calendar = Calendar.autoupdatingCurrent
```

**Calendar**类有常用的日期计算，比较和测试的方法。他能在日期组件和**Date**对象进行转换并兼顾不同的日历系统：

**你需要一个calendar对象转换日期组件和日期对象：**

```swift
let jan10 = calendar.date(from: dateComponents)
// jan 10, 2017, 9:30 AM
```

转换将返回一个可选的**Date**，如果是非法日期将返回**nil**。

为现有的日期设置特定的时间有一个方便的方法：

```swift
let today = Date()
let nineMorning = calendar.date(bySettingHour: 9, minute: 0, second: 0, of: today)
```

从一个日期获得一个或多个组件：

```swift
let weekOfYear = calendar.component(.weekOfYear, from: jan10!)
let monthAndYear = calendar.dateComponents([.month, .year], from: jan10!)
```

### **日期添加**

最安全的办法算术是使用用户的日历。例如：

```swift
let today = Date()

//明天相同的时间
let tomorrow = calendar.date(byAdding: .day, value: 1, to: today)

//一周后相同的时间
let oneWeekLater = calendar.date(byAdding: .day, value: 7, to: today)
```

**date(byAdding:value:to:)**方法返回一个可选日期，如果日期不存在将是nil。它尝试做正确的事情比如给1月的最后一天加上一个月得到2月的最后一天(如果存在闰月也是允许的)：

```swift
var jan31Components = DateComponents()
jan31Components.day = 31
jan31Components.month = 1
jan31Components.year = 2016  // A leap year

if let jan31 = calendar.date(from: jan31Components) {
  let nextMonth = calendar.date(byAdding: .month, value: 1, to: jan31, wrappingComponents: false)
  // Feb 29, 2016, 12:00 AM
}
```

###  查找下个日期

当你想找到下一个出现的日期,使用DateComponents对象指定你寻找的东西。例如找到下个星期一早上9点：

```swift
let today = Date()
var nextMonday = DateComponents()
nextMonday.weekday = 2
nextMonday.hour = 9
let result = calendar.nextDate(after: today, matching: nextMonday, matchingPolicy: .nextTime)
```

### 有用的测试

iOS8之后的一些有用的测试方法：

```swift
let today = Date()                // Nov 20, 2016, 7:00 PM
calendar.isDateInToday(today)     // true
calendar.isDateInWeekend(today)   // true
calendar.isDateInTomorrow(today)  // false
calendar.isDateInYesterday(today) // false
```

### 一日之始

因为iOS 8的方法让我的问题变得简单：

```swift
let today = Date()
// Nov 20, 2016, 7:00 PM

let startOfToday = calendar.startOfDay(for: today)
// Nov 20, 2016, 12:00 AM
```

### 总结起来

带着这些知识我们可以回到我原来的问题。我怎么能算出一天的开始一个给定的天数从今天开始吗? iOS8之后我们有**startOfDay(for: Date)**方法，所以我们可以分两步解决问题：

1. 找出给定日期的天数
2. 找出给定的日期开始计算日期的天

我们甚至可以推广步骤1允许我们添加任何日历组件。为了方便我将扩展Calendar类。

```swift
extension Calendar {
  func startOfDay(byAdding component: Calendar.Component, value: Int, to date: Date, wrappingComponents: Bool = false) -> Date? {
    guard let newDate = self.date(byAdding: component, value: value, to: date, wrappingComponents: wrappingComponents) else {
      return nil
    }
    return self.startOfDay(for: newDate)
  }
}
```

例子，获取这个月的第一天：

```swift
let calendar = Calendar.current
let startInMonth = calendar.startOfDay(byAdding: .month, value: 1, to: Date())
```

获取5天以内的第一天：

```swift
let startInDays = calendar.startOfDay(byAdding: .day, value: 5, to: Date())
```

我们能添加第二个方法获取包含今天的几天通过调用第一个方法：

```swift
func startOfDay(in days: Int) -> Date? {
  return self.startOfDay(byAdding: .day, value: days, to: Date())
}
```

获取5天以内的第一天：

```swift
let startInFive = calendar.startOfDay(in: 5)
```

### 进一步阅读

我强烈推荐看WWDC 2013日期和时间计算。这是标记为macOS,但我认为现在API的完全可用在iOS 10(和iOS8以后)：

* [WWDC 2013 Session 227 - Solutions to Command Date and Time Challenges](https://developer.apple.com/videos/play/wwdc2013/227/)

  ​