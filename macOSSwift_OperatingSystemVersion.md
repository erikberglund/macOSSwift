# macOSSwift: OperatingSystemVersion

### Index

* [Initialize OperatingSystemVersion using a version string like "10.13.6"](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_OperatingSystemVersion.md#initialize-operatingsystemversion-using-a-version-string-like-10136)
* [Check if two OperatingSystemVersion are equal](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_OperatingSystemVersion.md#check-if-two-operatingsystemversion-are-equal)

## Initialize OperatingSystemVersion using a version string like "10.13.6"

```swift
extension OperatingSystemVersion {
    init?(versionString: String) {
        let versionStringArray = versionString.components(separatedBy: ".")

        guard let majorVersion = Int(versionStringArray[0]) else { return nil }

        var minorVersion = 0
        if 1 < versionStringArray.count {
            guard let minorVersionInt = Int(versionStringArray[1]) else { return nil }
            minorVersion = minorVersionInt
        }

        var patchVersion = 0
        if 2 < versionStringArray.count {
            guard let patchVersionInt = Int(versionStringArray[2]) else { return nil }
            patchVersion = patchVersionInt
        }

        self.init(majorVersion: majorVersion, minorVersion: minorVersion, patchVersion: patchVersion)
    }
}
```

## Check if two OperatingSystemVersion are equal

```swift
extension OperatingSystemVersion {
    static func == (lhs: OperatingSystemVersion, rhs: OperatingSystemVersion) -> Bool {
        return
            lhs.majorVersion == rhs.majorVersion &&
            lhs.minorVersion == rhs.minorVersion &&
            lhs.patchVersion == rhs.patchVersion
    }
        
    static func != (lhs: OperatingSystemVersion, rhs: OperatingSystemVersion) -> Bool {
        return !(lhs == rhs)
    }
}