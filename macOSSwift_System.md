# macOSSwift: System

### Index

* [Boot Time](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_System.md#boot-time)
* [OS Version](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_System.md#os-version)
* [OS Build Version](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_System.md#os-build-version)

## Boot Time

This function will return the kernel (system) boot date and time.

```swift
var kernelBootTime: Date? {
    
    var mib = [ CTL_KERN, KERN_BOOTTIME ]
    var bootTime = timeval()
    var bootTimeSize = MemoryLayout<timeval>.stride
    
    if 0 != Darwin.sysctl(&mib, UInt32(mib.count), &bootTime, &bootTimeSize, nil, 0) {
        Swift.print("sysctl error: Could not get HW_MEMSIZE, errno: \(errno)")
        return nil
    }
    
    return Date(timeIntervalSince1970: TimeInterval(bootTime.tv_sec))
}
```

## OS Version

This function will return the operating system version of the running system as a string.

```
var osVersion: String {
    let osVersion = ProcessInfo().operatingSystemVersion
    return String(osVersion.majorVersion) + "." + String(osVersion.minorVersion) + "." + String(osVersion.patchVersion)
}
```

## OS Build Version

This function will return the operating system build version of the running system.

```swift
var osBuildVersion: String? {
    let root = IORegistryGetRootEntry(kIOMasterPortDefault)
    guard let osBuildVersion = IORegistryEntryCreateCFProperty(root, kOSBuildVersionKey as CFString, kCFAllocatorDefault, 0)?.takeRetainedValue() as? String else { return nil }
    return osBuildVersion
}
```
