# macOSSwift: OperatingSystemVersion

### Index

* [buildVersion](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_OperatingSystemVersion.md#buildversion)
* [init(versionString:)](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_OperatingSystemVersion.md#initialize-operatingsystemversion-using-a-version-string-like-10136)
* [Equatable](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_OperatingSystemVersion.md#equatable)

## init(versionString:)

Initialize OperatingSystemVersion using a version string like "10.13.6"

From [Foundation/ProcessInfo](https://github.com/apple/swift-corelibs-foundation/blob/5ffa3c88dcfdc3db73a7fcd7f1ca27611f59a0ef/Foundation/ProcessInfo.swift#L142-L146)

```swift
extension OperatingSystemVersion {
    init?(versionString: String) {
        let versionComponents = versionString.split(separator: ".").map(String.init).compactMap({ Int($0) })
        let majorVersion = versionComponents.dropFirst(0).first ?? fallbackMajor
        let minorVersion = versionComponents.dropFirst(1).first ?? fallbackMinor
        let patchVersion = versionComponents.dropFirst(2).first ?? fallbackPatch
        self.init(majorVersion: majorVersion, minorVersion: minorVersion, patchVersion: patchVersion)
    }
}
```

## buildVersion

Using `IORegistryEntryCreateCFProperty`

```swift
extension OperatingSystemVersion {
    public var buildVersion: String? {
        get {
            return IORegistryEntryCreateCFProperty(IORegistryGetRootEntry(kIOMasterPortDefault),
                                                   kOSBuildVersionKey as CFString,
                                                   kCFAllocatorDefault, 0)?.takeRetainedValue() as? String
        }
    }
}
```

Using `sysctlbyname`

```swift
extension OperatingSystemVersion {
    public var buildVersion: String? {
        get {
            let key = "kern.osversion"
            var size: Int = 0
            sysctlbyname(key, nil, &size, nil, 0)
            
            guard size > 0 else { return nil }
            
            var value = Array<CChar>(repeating: 0, count: size)
            sysctlbyname(key, &value, &size, nil, 0)
            
            return String(cString: value)
        }
    }
}
```

## Equatable

Conform OperatingSystemVersion to the Equatable protocol

```swift
extension OperatingSystemVersion: Equatable {
    
    public static func == (lhs: OperatingSystemVersion, rhs: OperatingSystemVersion) -> Bool {
        return lhs.majorVersion == rhs.majorVersion && lhs.minorVersion == rhs.minorVersion && lhs.patchVersion == rhs.patchVersion
    }
    
    public static func != (lhs: OperatingSystemVersion, rhs: OperatingSystemVersion) -> Bool {
        return !(lhs == rhs)
    }
}
```