src/features/devices/contexts/DeviceContext.tsx
根据 url 中的 query 中的 deviceId 字符串，来获取此设备的信息。
提供`device` 此设备信息（对象）、`refreshDevice` 加载设备信息并更新最新设备列表的方法（函数）、`updateDevice` 更新设备的方法（函数）。

-------
### Type Models
- `FreedomDevice` from lib/FreedomSDK/models/device-models
- `FreedomUser` from lib/FreedomSDK/models/user-models


### Data from other Context
- `activeAccountId` from `AppContext`


### Device Context
`DeviceContext` exposes states:
- `device` object
- `updateDevice` function
- `refreshDevice` function

### Device Context Provider
Device Context Provider receives some props (destructured):
- `children`
- `isShareLink` boolan
- `onDeviceLoad` function

#### const 
- token `publicToken` from `router.query.public_token` (string)
	- [[router]]
-  account id `activeAccountId` from `AppContext`
- session `session` created from `useSession()`
- cookie `recentDeviceCookie` from `useCookie(RECENT_DEVICES_COOKIE, '')` (a custom hook from features/app/hooks, cookie constant from features/app/constants/cookie-name-constants [[app-constants]])
- state `deviceInfo` created from `useState()`
- `deviceId` from `router.query`
- `saveDevice` renamed as `missionControlSaveDevice` from `useMissionControlUpdate()` (a custom hook from features/app/hooks)

#### 方法 functions
##### loadDeviceInfo
Purpose: 
- When `router.query.deviceId` exist, get device details.
- When is not share link visiter, keep track of the recent devices list.
Arguments: N/A

Only when `router.query.deviceId` exist, get device details:
1. use `getDeviceDetails` function (a method provide by lib/FreedomSDK) to get device details, needs two arguments:
- deviceId
- user token
	- if `isShareLink` (Context Provider's prop) is `true`, use `publicToken`
	- otherwise, use `seesion.user`
2. save the device details to state `deviceInfo`

When is not share link visiter, keep track of the recent devices list:
use `updateRecentDevices` function (created in the file), need one argument:
- device details object

Call `onDeviceLoad()` at the end, to let the `DevicesLayout` component know when to stop showing the loading screen.

##### updateRecentDevices (called by previous function)
Purpose: parsing the `recentDevicesCookie`, update the recent devices list
Arguments: deviceDetails object (type FreedomDevice)

const used in this function:
- `recentDevices` : an array parsed `JSON.parse` from cookie `recentDeviceCookie`
- `recentDeviceURL`: prepare URL using `activeAccountId` (from AppContext) and `deviceId` (from router.query)
- `recentDeviceName`: get the device name from `deviceDetails`

如果 `recentDevices` 数组中没有发现与当前设备名字相同的，那么把当前设备信息（url 和 name）添加进此数组，移除数组末端的元素`pop()` ，添加新的到数组第一位 `unshift()`
如果数组中已经存在此设备名字，那么要移除原来的 `findIndex` `splice(index, 1)` ，添加新的到数组第一位 `unshift()`
最后更新 cookie，通过调用 `updateRecentDevicesCookie` (从`useCookie`获取到的方法)，传递进参数：`recentDevices`
（原本 `recentDevices` 就是从 cookie 里提取出来的数组）

#### state
一个对象，属性与 DeviceContext 一样，state 在 Context Provider 里用来 更新 数据。在 useEffect 中被更新。
- device
- refreshDevice
- updateDevice: 调用了一个来自 lib/FreedomSDK/methods 的方法 `updateDevice()` ，返回 `loadDeviceInfo()` 函数。

### useEffect
根据 `deviceInfo` 运行。
如果 `deviceInfo` 存在（loadDeviceInfo 成功保存设备信息到`deviceInfo` 这个 state），push recent device for mission control，同时更新上面的代表 DeviceContext 的 state。

### useEffect
根据 `deviceId` 运行。
调用 `loadDeviceInfo` 方法。