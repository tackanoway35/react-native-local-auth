# react-native-local-auth

Authenticate users with Touch ID, with optional fallback to passcode (if TouchID is unavailable or not enrolled). Most of the code and documentation is originally from [react-native-touch-id](https://github.com/naoufal/react-native-touch-id), but together with [naoufal](https://github.com/naoufal) we decided that fallback to passcode didn't belong in [react-native-touch-id](https://github.com/naoufal/react-native-touch-id).

## Documentation
- [UI](https://github.com/tradle/react-native-local-auth#ui)
- [Install](https://github.com/tradle/react-native-local-auth#install)
- [Usage](https://github.com/tradle/react-native-local-auth#usage)
- [Errors](https://github.com/tradle/react-native-local-auth#errors)
- [License](https://github.com/tradle/react-native-local-auth#license)

## UI

If TouchID is supported and enrolled (in this gif, 1st touch fails, 2nd succeeds)

![Touch ID](gifs/touchID.gif)

If TouchID is not supported or not enrolled, you can fallback to device passcode

![fallback to passcode](gifs/fallback to passcode.gif)

## Install
```shell
npm i --save react-native-local-auth
```

### Linking the Library
First link the library to your project.  There's excellent documentation on how to do this in the [React Native Docs](http://facebook.github.io/react-native/docs/linking-libraries-ios.html#content).

#### Android

1. Open up `android/app/src/main/java/[...]/MainActivity.java`
  - Add `import io.tradle.react.LocalAuthPackage;` to the imports at the top of the file
  - Add `new LocalAuthPackage()` to the list returned by the `getPackages()` method
2. Append the following lines to `android/settings.gradle`:
    ```
    include ':react-native-local-auth'
    project(':react-native-local-auth').projectDir = new File(rootProject.projectDir,   '../node_modules/react-native-local-auth/android')
    ```
3. Insert the following lines inside the dependencies block in `android/app/build.gradle`:
    ```
      compile project(':react-native-local-auth')
    ```

## Usage

```js
var LocalAuth = require('react-native-local-auth')

var YourComponent = React.createClass({
  _pressHandler() {
    LocalAuth.authenticate({
        reason: 'this is a secure area, please authenticate yourself',
        falbackToPasscode: true,    // fallback to passcode on cancel
        suppressEnterPassword: true // disallow Enter Password fallback
      })
      .then(success => {
        AlertIOS.alert('Authenticated Successfully')
      })
      .catch(error => {
        AlertIOS.alert('Authentication Failed', error.message)
      })
  },

  render() {
    return (
      <View>
        ...
        <TouchableHighlight onPress={this._pressHandler}>
          <Text>
            Authenticate with Touch ID / Passcode
          </Text>
        </TouchableHighlight>
      </View>
    )
  }
})
```

### hasTouchID()
check if Touch ID is supported.
Returns a `Promise` object.

## Errors
There are various reasons why authenticating with Touch ID or device passcode may fail.  Whenever authentication fails, `LocalAuth.authenticate` will return an error code representing the reason.

Below is a list of error codes that can be returned:

| Code | Description |
|---|---|
| `LAErrorAuthenticationFailed` | Authentication was not successful because the user failed to provide valid credentials. |
| `LAErrorUserCancel` | Authentication was canceled by the user—for example, the user tapped Cancel in the dialog. |
| `LAErrorUserFallback` | Authentication was canceled because the user tapped the fallback button (Enter Password). |
| `LAErrorSystemCancel` | Authentication was canceled by system—for example, if another application came to foreground while the authentication dialog was up. |
| `LAErrorPasscodeNotSet` | Authentication could not start because the passcode is not set on the device. |
| `LAErrorTouchIDNotAvailable` | Authentication could not start because Touch ID is not available on the device |
| `LAErrorTouchIDNotEnrolled` | Authentication could not start because Touch ID has no enrolled fingers. |
| `RCTTouchIDUnknownError` | Could not authenticate for an unknown reason. |
| `RCTTouchIDNotSupported` | Device does not support Touch ID. |

_More information on errors can be found in [Apple's Documentation](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/c/tdef/LAError)._

## License

ISC. [react-native-touch-id](https://github.com/naoufal/react-native-touch-id) license also included.
