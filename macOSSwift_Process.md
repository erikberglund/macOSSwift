# macOSSwift: Process

### Index

* [Parent PID](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_Process.md#parent-pid)

## Parent PID

This func will return the parent PID of the PID passed.

```swift
func getParentPID(of pid: pid_t) -> pid_t? {

    var mib = [ CTL_KERN, KERN_PROC, KERN_PROC_PID, pid]
    var kinfo = kinfo_proc()
    var kinfoSize = MemoryLayout<kinfo_proc>.stride
   
    if 0 != Darwin.sysctl(&mib, UInt32(mib.count), &kinfo, &kinfoSize, nil, 0) {
        Swift.print("sysctl error: Could not get KERN_PROC_PID, errno: \(errno)")
        return nil
    }

    return kinfo.kp_eproc.e_ppid
}
```
