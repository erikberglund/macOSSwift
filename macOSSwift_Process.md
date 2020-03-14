# macOSSwift: Process

### Index

* [Parent PID](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_Process.md#parent-pid)

## Parent PID

This func will return the parent PID of the PID passed.

```swift
func getParentPID(of pid: pid_t) -> pid_t? {

    var mib = [ CTL_KERN, KERN_PROC, KERN_PROC_PID, pid ]
    var kinfo_proc = kinfo_proc()
    var kinfo_procSize = MemoryLayout<kinfo_proc>.stride
   
    if 0 != Darwin.sysctl(&mib, UInt32(mib.count), &kinfo_proc, &kinfo_procSize, nil, 0) {
        Swift.print("sysctl error: Could not get KERN_PROC_PID, errno: \(errno)")
        return nil
    }

    return kinfo_proc.kp_eproc.e_ppid
}
```
