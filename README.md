#  NaiveDateTime

<p align="left">
<img src="https://img.shields.io/cocoapods/v/NaiveDateTime.svg?label=version">
<img src="https://img.shields.io/badge/supports-CocoaPods%20%7C%20Carthage%20%7C%20SwiftPM-green.svg">
<img src="https://img.shields.io/cocoapods/p/NaiveDateTime.svg?style=flat)">
<a href="https://travis-ci.org/kean/NaiveDateTime"><img src="https://img.shields.io/travis/kean/NaiveDateTime/master.svg"></a>
</p>

The missing **naive** date, time, and datetime types.


## Usage

Native `Date` type is great for working with dates along the time zones (e.g. "2017-09-29T15:00:00+0300"), however there are some scenarios in which naive dates and times are desirable.

The library implements three types for representing naive date:
- `NaiveDate` (e.g. "2017-09-29")
- `NaiveTime` (e.g. "15:30:00")
- `NaiveDateTime` (e.g. "2017-09-29T15:30:00" - no time zone and no offset).

Each of the provided types implements `Equatable`, `Comparable`, `LosslessStringConvertible`, `Codable` protocols, and can also be converted to  `Date`, and `DateComponents`.

**Important!** The naive types do not perform any validation of the input components (year, hour, etc). If you do need to do any precise manipulations with time use native `Date` and `Calendar` types.

### Create

Naive dates and times can be created either from strings (using a predefined format) or by using a memberwise method:

```swift
NaiveDate("2017-10-01")
NaiveDate(year: 2017, month: 10, day: 1)

NaiveTime("15:30:00")
NaiveTime(hour: 15, minute: 30, second: 0)

NaiveDateTime("2017-10-01T15:30")
NaiveDateTime(
    date: NaiveDate(year: 2017, month: 10, day: 1),
    time: NaiveTime(hour: 15, minute: 30, second: 0)
)
```

### Format

Format dates without having to worry about time zones:

```swift
let date = NaiveDate("2017-11-01")!
NaiveDateFormatter(dateStyle: .short).string(from: date)
// prints "11/1/17"

let time = NaiveTime("15:00")!
NaiveDateFormatter(timeStyle: .short).string(from: time)
// prints "3:00 PM"

let dateTime = NaiveDateTime("2017-11-01T15:30:00")!
NaiveDateFormatter(dateStyle: .short, timeStyle: .short).string(from: dateTime)
// prints "11/1/17, 3:30 PM"
```

### Convert

When you do need time zones, convert `NaiveDate` to `Date`:

```swift
let date = NaiveDate(year: 2017, month: 10, day: 1)

// Creates `Date` in a calendar's time zone
// "2017-10-01T00:00:00+0300" if user is in MSK
Calendar.current.date(from: date)

// Creates `Date` with +0000 offset
// "2017-10-01T00:00:00Z"
Calendar.current.date(from: date, in: TimeZone(secondsFromGMT: 0)!)
```

```swift
let dateTime = NaiveDateTime(
    date: NaiveDate(year: 2017, month: 10, day: 1),
    time: NaiveTime(hour: 15, minute: 30, second: 0)
)

// Creates `Date` in a calendar's time zone
// "2017-10-01T15:30:00+0300" if user is in MSK
Calendar.current.date(from: dateTime)

// Creates `Date` with +0000 offset
// "2017-10-01T15:30:00Z"
Calendar.current.date(from: dateTime, in: TimeZone(secondsFromGMT: 0)!),
```


## Requirements

- iOS 10.0 / watchOS 3.0 / OS X 10.12 / tvOS 10.0
- Xcode 9
- Swift 4


## License

NaiveDateTime is available under the MIT license. See the LICENSE file for more info.