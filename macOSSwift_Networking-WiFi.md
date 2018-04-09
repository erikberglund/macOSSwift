# macOSSwift: Networking - WiFi

# Remove a SSID from the preferred wireless network list

```swift
import CoreWLAN
import SecurityFoundation

func removePreferredWirelessNetwork(ssid: String) throws {
	if let interfaces = CWWiFiClient.shared().interfaces() {
		for interface in interfaces {
			
			// Get the configuration for the current interface
			guard let interfaceConfiguration = interface.configuration() else { continue }
			
			// Get an array of all preferred wireless networks for the current interface
			guard var networkProfiles = interfaceConfiguration.networkProfiles.array as? [CWNetworkProfile] else { continue }
			
			// Get the index of the passed ssid (if one exists in the preferred wireless networks array)
			if let index = networkProfiles.index(where: { $0.ssid == ssid }) {
			
				// Remove the CWNetworkProfile from the networkProfiles array
				networkProfiles.remove(at: index)
				
				// Create a CWMutableConfiguration from the interface configuration
				let mutableInterfaceConfiguration = CWMutableConfiguration(configuration: interfaceConfiguration)
				
				// Update the preferred networks list in the configuration
				mutableInterfaceConfiguration.networkProfiles = NSOrderedSet(array: networkProfiles)
				
				// Update the configuration for the current interface
				// Authentication is required if the application is not running as root
				try interface.commitConfiguration(mutableInterfaceConfiguration, authorization: nil)
			}
		}
	}
}
```
