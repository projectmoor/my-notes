This Hook will pull down all the devices within the account, and will be the place where we should reference shared

Unique keys such as device Types, and Groups. We can use this hook to centralize the retrieval of devices data.

-------

## import
`getDevices` from lib/FreedomSDK/methods/account/get-devices


## return
This hook returns object in the form of `useDevicesDataValues` type.
an object includes properties
- devices: an FreedomDevice array
- deviceTypesOptions: array
- deviceGroupsOptions: array
- deviceConnectionStatusOptions: array
- isLoadingDevices: boolean
- refreshDevicesData: function

## useDevicesData's props
- Optional: interval$ is type of Observable, which is a class defined by rxjs library [[My Questions]]
- Optional: onInitialLoad is a function


## useDevicesData Component
`useToasts()` hook from geist: show an globally message on the lower right corner. You can even add a button to the message.
You could show more information by replacing the string with a `ReactNode`.

