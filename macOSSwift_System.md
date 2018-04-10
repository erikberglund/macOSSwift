# macOSSwift: System

### Index

* [Boot Time](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_System.md#boot-time)
* [RAM Installed](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_System.md#ram-installed)

## Boot Time

This function will return the kernel (system) boot date and time.

```swift
func kernelBootTime() -> Date {
    
    var mib = [ CTL_KERN, KERN_BOOTTIME ]
    var bootTime = timeval()
    var bootTimeSize = MemoryLayout<timeval>.stride
    
    if 0 != sysctl(&mib, UInt32(mib.count), &bootTime, &bootTimeSize, nil, 0) {
        fatalError("Could not get boot time, errno: \(errno)")
    }
    
    return Date(timeIntervalSince1970: TimeInterval(bootTime.tv_sec))
}
```

## RAM Installed

This function will return the currently installed RAM in bytes.

```swift
func physicalMemory() -> Int64 {
    
    var mib: [Int32] = [ CTL_HW, HW_MEMSIZE ]
    var physicalMemory: Int64 = 0
    var physicalMemorySize = MemoryLayout<Int64>.stride
    
    if 0 != sysctl(&mib, UInt32(mib.count), &physicalMemory, &physicalMemorySize, nil, 0) {
        fatalError("Could not get boot time, errno: \(errno)")
    }
    
    return physicalMemory
}
```
