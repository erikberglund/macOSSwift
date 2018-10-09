# macOSSwift: System

### Index

* [Boot Time](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_System.md#boot-time)
* [Hardware Model](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_System.md#hardware-model)
* [RAM Installed](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_System.md#ram-installed)
* [Serial Number](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_System.md#serial-number)
* [Configuration Code](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_System.md#configuration-code)

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

## Hardware Model

This function will return the hardware model of the machine running the code.

```swift
var hardwareModel: String? {
    
    var mib = [ CTL_HW, HW_MODEL ]
    var count = 0
    
    guard 0 == Darwin.sysctl(&mib, 2, nil, &count, nil, 0) else {
        Swift.print("sysctl error: Could not get HW_MODEL size, errno: \(errno)")
        return nil
    }
    
    var hardwareModel = [CChar](repeating: 0, count: count)
    guard 0 == Darwin.sysctl(&mib, 2, &hardwareModel, &size, nil, 0) else {
        Swift.print("sysctl error: Could not get HW_MODEL, errno: \(errno)")
        return nil
    }
    
    return String(cString: hardwareModel)
}
```

## RAM Installed

This function will return the currently installed RAM in bytes.

```swift
var physicalMemory: Int64? {
    
    var mib = [ CTL_HW, HW_MEMSIZE ]
    var physicalMemory: Int64 = 0
    var physicalMemorySize = MemoryLayout<Int64>.stride
    
    if 0 != Darwin.sysctl(&mib, UInt32(mib.count), &physicalMemory, &physicalMemorySize, nil, 0) {
        Swift.print("sysctl error: Could not get HW_MEMSIZE, errno: \(errno)")
        return nil
    }
    
    return physicalMemory
}
```

## Serial Number

This var will return the machine serial number.

```swift  
var machineSerialNumber: String? {
    let platformExpert = IOServiceGetMatchingService(kIOMasterPortDefault, IOServiceMatching("IOPlatformExpertDevice") )
    guard 0 < platformExpert else { return nil }
    guard let platformExpertSN = IORegistryEntryCreateCFProperty(platformExpert,
                                                                 kIOPlatformSerialNumberKey as CFString,
                                                                 kCFAllocatorDefault, 0).takeUnretainedValue() as? String else { return nil }
    IOObjectRelease(platformExpert)
    return platformExpertSN.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines)
}
```

## Configuration Code

This var will return the machine configuration code (i.e the last 4 characters of the machine serial number)

```swift
var machineConfigurationCode: String? {
    guard let sn = machineSerialNumber, (sn.count == 11 || sn.count == 12) else { return nil }
    return String(sn.suffix(4))
}
```
