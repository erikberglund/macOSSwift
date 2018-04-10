# macOSSwift: System

### Index

* [Boot Time](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_System.md#boot-time)

## Boot Time

This function will return the kernel (system) boot date and time.

```swift
func kernelBootTime() -> Date {
    
    var mib = [ CTL_KERN, KERN_BOOTTIME ]
    var bootTime = timeval()
    var bootTimeSize = MemoryLayout<timeval>.size
    
    if 0 != sysctl(&mib, UInt32(mib.count), &bootTime, &bootTimeSize, nil, 0) {
        fatalError("Could not get boot time, errno: \(errno)")
    }
    
    return Date(timeIntervalSince1970: TimeInterval(bootTime.tv_sec))
}
```
