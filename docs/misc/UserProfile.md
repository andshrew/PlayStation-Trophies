## Basic User Profile

        https://us-prof.np.community.playstation.net/userProfile/v1/users/{onlineId}/profile2

In addition to returning basic information about a PSN account, this endpoint also allows you to find an accounts `accountId` directly from the accounts username (`onlineId`). This is useful as many other endpoints which query another PSN account depend on querying against the `accountId`.  

The endpoint can query the authenticating account, or it can query another user account (providing that the privacy settings on the other account allow it).  

### Input Parameters

| Parameter | Type | Example Value | Description | Required |
| --- | --- | --- | --- | --- |
| onlineId | String | `me`<br>`username` | The username of the account to query<br>Use `me` to query the authenticating account | Yes
| fields | String | `accountId,onlineId,currentOnlineId` | Comma separated list of account attributes to return<br>Some are encapsulated within `@attribute(attribute1, attribute2)` notion | No

<details>
  <summary>Click to view additional `fields` parameter details</summary>
  Examples of other fields will be added here.
</details>

### Output JSON Response

A JSON response is returned. The following are returned under the `profile` attribute (dependant on which fields are supplied as the input parameter).

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- |
| onlineId | String | `username` | The username of the account being accessed
| accountId | Numeric | `12340..` | The ID of the account being accessed
| currentOnlineId | String | `username` | If you query the original username of an account that has since changed its name, then this field is returned to tell you what the current username is

### Example URLs and Responses

#### Example 1 - Profile of the authenticating account <!-- {docsify-ignore} -->

    https://us-prof.np.community.playstation.net/userProfile/v1/users/me/profile2?fields=accountId,onlineId,currentOnlineId

```json
{
  "profile": {
    "onlineId": "username",
    "accountId": "0000000000000000000"
  }
}
```
Executing this example using Powershell - see [Querying the API](/APIv2?id=powershell-7)
```powershell
Invoke-RestMethod -Uri "https://us-prof.np.community.playstation.net/userProfile/v1/users/me/profile2?fields=accountId,onlineId,currentOnlineId" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```

#### Example 2 - Profile of user account "example" which had subsequently changed username to "newexample" <!-- {docsify-ignore} -->

    https://us-prof.np.community.playstation.net/userProfile/v1/users/example/profile2?fields=accountId,onlineId,currentOnlineId

```json
{
  "profile": {
    "onlineId": "example",
    "accountId": "0000000000000000000",
    "currentOnlineId": "newexample"
  }
}
```
Executing this example using Powershell - see [Querying the API](/APIv2?id=powershell-7)
```powershell
Invoke-RestMethod -Uri "https://us-prof.np.community.playstation.net/userProfile/v1/users/example/profile2?fields=accountId,onlineId,currentOnlineId" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```

## Alternative User Profile

        https://m.np.playstation.com/api/userProfile/v1/internal/users/{accountId}/profiles

An alternative profile endpoint to the above, it retrieves a more limited set of information for an account and it requires knowing the `{accountId}` rather than the `{onlineId}`. However, unlike the above example, this is an endpoint which is still actively used by the PS App and so may have greater longevity.

The endpoint can query the authenticating account, or it can query another user account (providing that the privacy settings on the other account allow it).  

### Input Parameters

| Parameter | Type | Example Value | Description | Required |
| --- | --- | --- | --- | --- |
| accountId | Numeric | `12340..` | The ID of the account to query | Yes

### Output JSON Response

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- |
| onlineId | String | `username` | The username of the account being accessed
| personalDetail | JSON Object | | Contains the users real name, and URLs to the custom profile picture if they have set one. Only returned for the authenticating account, or if queried account privacy settings allow it
| aboutMe | String | `Hello there` | A user defined description field (up to 140 characters)<br>An empty string if not set
| avatars | JSON Object | | URLs for the users avatar in various sizes (`s`, `m`, `l`, `xl`)
| languages | JSON Object | `en-us` | Up to three languages that the user has selected for their account
| isPlus | Boolean | `true` | `true` when user is a PS+ member
| isOfficiallyVerified | Boolean | `true` | `true` when user is a VIP
| isMe | Boolean | `true` | `true` if the queried user is the authenticating account

### Example URLs and Responses

#### Example 1 - Profile of the authenticating account with accountId _0000000000000000000_ <!-- {docsify-ignore} -->

    https://m.np.playstation.com/api/userProfile/v1/internal/users/0000000000000000000/profiles

```json
{
  "onlineId": "ExampleUser",
  "personalDetail": {
    "firstName": "Example",
    "lastName": "User"
  },
  "aboutMe": "",
  "avatars": [
    {
      "size": "s",
      "url": "http://static-resource.np.community.playstation.net/avatar_s/WWS_J/IP90001008001s.png"
    },
    {
      "size": "xl",
      "url": "http://static-resource.np.community.playstation.net/avatar_xl/WWS_J/IP90001008001_XL.png"
    },
    {
      "size": "l",
      "url": "http://static-resource.np.community.playstation.net/avatar/WWS_J/IP90001008001l.png"
    },
    {
      "size": "m",
      "url": "http://static-resource.np.community.playstation.net/avatar_m/WWS_J/IP90001008001m.png"
    }
  ],
  "languages": [
    "en-US"
  ],
  "isPlus": false,
  "isOfficiallyVerified": false,
  "isMe": true
}
```
Executing this example using Powershell - see [Querying the API](/APIv2?id=powershell-7)
```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.com/api/userProfile/v1/internal/users/0000000000000000000/profiles" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```

#### Example 2 - Profile of a verified VIP account account with accountId _0000000000000000000_ <!-- {docsify-ignore} -->

    https://m.np.playstation.com/api/userProfile/v1/internal/users/0000000000000000000/profiles

```json
{
  "onlineId": "yosp",
  "personalDetail": {
    "displayName": "Shuhei Yoshida",
    "profilePictures": [
      {
        "size": "s",
        "url": "https://image.api.np.km.playstation.net/images/?format=png&w=50&h=50&image=https%3A%2F%2Fkfscdn.api.np.km.playstation.net%2Faf0e927a83a156e3ca65f2b7c1209768ce359d7b56882ccfa1d42b50bbffc0a9%2Fccf1c3ecd834e5af6da17fc7c67acc3d%2F1412032653104.png"
      },
      {
        "size": "m",
        "url": "https://image.api.np.km.playstation.net/images/?format=png&w=160&h=160&image=https%3A%2F%2Fkfscdn.api.np.km.playstation.net%2Faf0e927a83a156e3ca65f2b7c1209768ce359d7b56882ccfa1d42b50bbffc0a9%2Fccf1c3ecd834e5af6da17fc7c67acc3d%2F1412032653104.png"
      },
      {
        "size": "l",
        "url": "https://image.api.np.km.playstation.net/images/?format=png&w=240&h=240&image=https%3A%2F%2Fkfscdn.api.np.km.playstation.net%2Faf0e927a83a156e3ca65f2b7c1209768ce359d7b56882ccfa1d42b50bbffc0a9%2Fccf1c3ecd834e5af6da17fc7c67acc3d%2F1412032653104.png"
      },
      {
        "size": "xl",
        "url": "https://image.api.np.km.playstation.net/images/?format=png&w=440&h=440&image=https%3A%2F%2Fkfscdn.api.np.km.playstation.net%2Faf0e927a83a156e3ca65f2b7c1209768ce359d7b56882ccfa1d42b50bbffc0a9%2Fccf1c3ecd834e5af6da17fc7c67acc3d%2F1412032653104.png"
      }
    ]
  },
  "aboutMe": "Shuhei Yoshida",
  "avatars": [
    {
      "size": "s",
      "url": "http://static-resource.np.community.playstation.net/avatar_s/SCEI/IP91001401PR2_10DB033BEC5A1E6A2351_S.png"
    },
    {
      "size": "xl",
      "url": "http://static-resource.np.community.playstation.net/avatar_xl/SCEI/IP91001401PR2_50D89087FFA1BD35844B_XL.png"
    },
    {
      "size": "l",
      "url": "http://static-resource.np.community.playstation.net/avatar/SCEI/IP91001401PR2_BFFC2B8AD0E21BC6CE29_L.png"
    },
    {
      "size": "m",
      "url": "http://static-resource.np.community.playstation.net/avatar_m/SCEI/IP91001401PR2_99ED763F79B9E245F073_M.png"
    }
  ],
  "languages": [
    "ja-JP",
    "en-US"
  ],
  "isPlus": true,
  "isOfficiallyVerified": true,
  "isMe": false
}
```
Executing this example using Powershell - see [Querying the API](/APIv2?id=powershell-7)
```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.com/api/userProfile/v1/internal/users/0000000000000000000/profiles" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```

