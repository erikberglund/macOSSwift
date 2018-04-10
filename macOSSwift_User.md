# macOSSwift: User

### Index

* [LoginTime](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_User.md#login-time)

## Login Time

This function will return the last date and time the passed usernamed logged in.  
We're using the wtmp-database, so the data will rotate after some time.

```swift
func lastLoginDate(username: String) -> Date? {
    
    var lastLoginDate: Date?
    
    // Reset the database, so that the next getutxent() call will get the first entry.
    // Passing 0 will return entries in reverse chronological order
    setutxent_wtmp(0)
    
    // Loop through all entries in the database
    while let wtmpPointer = getutxent_wtmp() {
        var wtmpEntry = wtmpPointer.pointee
        
        // Ignore all entries that's not a console login
        let line = withUnsafeBytes(of: &wtmpEntry.ut_line) { (rawPtr) -> String in
            let ptr = rawPtr.baseAddress!.assumingMemoryBound(to: CChar.self)
            return String(cString: ptr)
        }
        if line != "console" { continue }
        
        // Ignore all entries that doesn't contain the current user
        let user = withUnsafeBytes(of: &wtmpEntry.ut_user) { (rawPtr) -> String in
            let ptr = rawPtr.baseAddress!.assumingMemoryBound(to: CChar.self)
            return String(cString: ptr)
        }
        if user != username { continue }
        
        // Ignore all entries that's not a login
        if wtmpEntry.ut_type == Int16(USER_PROCESS) {
            lastLoginDate = Date(timeIntervalSince1970: TimeInterval(wtmpEntry.ut_tv.tv_sec))
            break
        }
    }
    
    // Close the database
    endutxent_wtmp()
    
    return lastLoginDate
}
```
