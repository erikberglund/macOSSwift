# macOSSwift: OperatingSystemVersion

### Index

* [Initialize a OperatingSystemVersion instance using a version string (10.13.6)](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_OperatingSystemVersion.md#remove-an-ssid-from-the-preferred-wireless-network-list)

## Initialize a OperatingSystemVersion instance using a version string (10.13.6)

```swift
extension OperatingSystemVersion {
    init?(versionString: String) {
        let versionStringArray = versionString.components(separatedBy: ".")
        if versionStringArray.count < 1 { return nil }
        guard let majorVersion = Int(versionStringArray[0]) else { return nil }
        
		var minorVersion = 0
        if 1 < versionStringArray.count {
            if let minorVersionInt = Int(versionStringArray[1]) {
                minorVersion = minorVersionInt
            } else {
                return nil
            }
        }
        
        var patchVersion = 0
        if 2 < versionStringArray.count {
            if let patchVersionInt = Int(versionStringArray[2]) {
                patchVersion = patchVersionInt
            } else {
                return nil
            }
        }

        self.init(majorVersion: majorVersion, minorVersion: minorVersion, patchVersion: patchVersion)
    }
}

```
