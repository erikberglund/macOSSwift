# macOSSwift: SecCertificate

### Index

* [isValid](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_SecCertificate.md#isValid)

## isValid

This extension will return if the SecCertificate is valid for the current date.

```swift
extension SecCertificate {
    var isValid: Bool {
        let oids: [CFString] = [kSecOIDX509V1ValidityNotAfter, kSecOIDX509V1ValidityNotBefore]
        let certificateValueDict = SecCertificateCopyValues(self, oids as CFArray, nil) as? Dictionary<String, Dictionary<String, Any>>
        return relativeTime(forOID: kSecOIDX509V1ValidityNotAfter, values: certificateValueDict) >= 0.0
            && relativeTime(forOID: kSecOIDX509V1ValidityNotBefore, values: certificateValueDict) <= 0.0
    }
        
    func relativeTime(forOID oid: CFString, values: Dictionary<String, Dictionary<String, Any>>?) -> Double {
        guard let dateNumber = values?[oid as String]?[kSecPropertyKeyValue as String] as? NSNumber else { return 0.0 }
        return dateNumber.doubleValue - CFAbsoluteTimeGetCurrent()
    }
}
```
