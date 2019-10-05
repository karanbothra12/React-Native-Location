# React-Native-Location

Installation
yarn

yarn add react-native-geolocation-service
npm

npm install react-native-geolocation-service
Compatibility
RN Version	Package Version
0.60+	3.0.0
0.57+	2.0.0
<0.57	1.1.0
Setup
iOS
You need to include the NSLocationWhenInUseUsageDescription key in Info.plist to enable geolocation when using the app. In order to enable geolocation in the background, you need to include the NSLocationAlwaysUsageDescription key in Info.plist and add location as a background mode in the 'Capabilities' tab in Xcode.

NOTE: This library uses @react-native-community/geolocation under the hood for iOS. It'll be installed along with this library, the following instruction describes how to integrate it in your project.

0.60 or higher
0.59 or below
Android
No additional setup is required for 0.60 or above.

0.59 or below
Usage
Since this library was meant to be a drop-in replacement for the RN's Geolocation API, the usage is pretty straight forward, with some extra error cases to handle.

One thing to note, this library assumes that location permission is already granted by the user, so you have to use PermissionsAndroid to request for permission before making the location request.

...
import Geolocation from 'react-native-geolocation-service';
...

componentDidMount() {
    // Instead of navigator.geolocation, just use Geolocation.
    if (hasLocationPermission) {
        Geolocation.getCurrentPosition(
            (position) => {
                console.log(position);
            },
            (error) => {
                // See error code charts below.
                console.log(error.code, error.message);
            },
            { enableHighAccuracy: true, timeout: 15000, maximumAge: 10000 }
        );
    }
}
API
setRNConfiguration(options) (iOS only)
options:

Name	Type	Default	Description
skipPermissionRequests	bool	false	If true, you must request permissions before using Geolocation APIs.
authorizationLevel	string	--	Changes whether the user will be asked to give "always" or "when in use" location services permission. Any other value or auto will use the default behaviour, where the permission level is based on the contents of your Info.plist. Possible values are whenInUse, always and auto.
requestAuthorization() (iOS only)
Request suitable Location permission based on the key configured on pList. If NSLocationAlwaysUsageDescription is set, it will request Always authorization, although if NSLocationWhenInUseUsageDescription is set, it will request InUse authorization.

getCurrentPosition(successCallback, ?errorCallback, ?options)
successCallback: Invoked with latest location info.

errorCallback: Invoked whenever an error is encountered.

options:

Name	Type	Default	Description
timeout	ms	INFINITY	Request timeout
maximumAge	ms	INFINITY	How long previous location will be cached
enableHighAccuracy	bool	false	Use high accuracy mode
distanceFilter	m	0	Minimum displacement in meters
showLocationDialog	bool	true	Whether to ask to enable location in Android (android only)
forceRequestLocation	bool	false	Force request location even after denying improve accuracy dialog (android only)
useSignificantChanges	bool	false	Uses the battery-efficient native significant changes APIs to return locations. Locations will only be returned when the device detects a significant distance has been breached (iOS only)
watchPosition(successCallback, ?errorCallback, ?options)
successCallback: Invoked with latest location info.

errorCallback: Invoked whenever an error is encountered.

options:

Name	Type	Default	Description
enableHighAccuracy	bool	false	Use high accuracy mode
distanceFilter	m	100	Minimum displacement between location updates in meters
interval	ms	10000	Interval for active location updates (android only)
fastestInterval	ms	5000	Fastest rate at which your application will receive location updates, which might be faster than interval in some situations (for example, if other applications are triggering location updates) (android only)
showLocationDialog	bool	true	whether to ask to enable location in Android (android only)
forceRequestLocation	bool	false	Force request location even after denying improve accuracy dialog (android only)
useSignificantChanges	bool	false	Uses the battery-efficient native significant changes APIs to return locations. Locations will only be returned when the device detects a significant distance has been breached (iOS only)
clearWatch(watchId)
watchId (id returned by watchPosition)
stopObserving()
Stops observing for device location changes. In addition, it removes all listeners previously registered.
