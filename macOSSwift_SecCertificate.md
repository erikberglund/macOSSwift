# macOSSwift: SecCertificate

### Index

* [isSelfSigned](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_SecCertificate.md#isselfsigned)
* [isValid](https://github.com/erikberglund/macOSSwift/blob/master/macOSSwift_SecCertificate.md#isvalid)

## isSelfSigned

This extension will return if the SecCertificate is self signed.

```swift
extension SecCertificate {

    var isSelfSigned: Bool {
        if #available(OSX 10.12.4, *) {
            return SecCertificateCopyNormalizedIssuerSequence(self) == SecCertificateCopyNormalizedSubjectSequence(self)
        } else {
            var errorRef : Unmanaged<CFError>?
            let issuerData = SecCertificateCopyNormalizedIssuerContent(self, &errorRef)
            let issuerDataError = errorRef?.takeRetainedValue()
            
            if issuerData == nil {
                Swift.print("Failed to get issuer data with error: \(String(describing: issuerDataError))")
            }
            
            let subjectData = SecCertificateCopyNormalizedSubjectContent(self, &errorRef)
            let subjectDataError = errorRef?.takeRetainedValue()
            
            if subjectData == nil {
                Swift.print("Failed to get subject data with error: \(String(describing: subjectDataError))")
            }
            
            return issuerData == subjectData
        }
    }
}
```

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
