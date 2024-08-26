## Console Storage Usage

        https://m.np.playstation.com/api/cloudAssistedNavigation/v2/users/me/clients

This endpoint returns a list of installed content on your console. It works with PS5, and _may_ work with PS4 storage too. It _may_ also require that you have your Data Sharing settings with Sony set to Full rather than Limited.  

It includes information such as what content is installed, where it is installed (ie. internal, USB, M.2), and how much space it occupies in bytes.  

The endpoint can only query the authenticating account.  

### Input Parameters

| Parameter | Type | Example Value | Description | Required |
| --- | --- | --- | --- | --- |
| includeFields | String | `device,systemData` | Comma separated list of fields to return | Yes
| platform | String | `PS5` | Console type | Yes

#### Additional Headers <!-- {docsify-ignore} -->

Requests to this endpoint must include additional headers in the request.

| Key | Value |
| --- | --- |
| Accept-Language | `en-gb`<br>`en-us`<br>`de-de` |

### Output JSON Response

A JSON response is returned. As a large amount of information is returned that is mostly self explanatory, I've only included observations here that may be of interest.  

Under the `systemData` attribute, the title data returned seems to belong to content that has been streamed via PS+ Premium. The `lastAccessDateTime` does not seem to be accurate to when the game was last streamed.  

Under the `installedTitles` attribute, the title data returned seems to belong to content that is actually installed on the console. Unlike the above, `lastAccessDateTime` does seem to be accurate, however it may be influenced by when the console performs an auto update check on the title (or even other factors like viewing the games title in the library) and so doesn't seem to be when the title was last launched.  

The `version` for PS4 content does not seem to be accurate, almost all of mine are listed as version `01.00`. The version numbers for PS5 content do seem to be accurate.  

Under the `storage` attribute, `embedded` is the PS5 built in storage, `mounted` is external USB storage, and `m2Extended` is the M.2 drive in the expansion slot.

### Example URL and Response

        https://m.np.playstation.com/api/cloudAssistedNavigation/v2/users/me/clients?includeFields=device%2CsystemData&platform=PS5

```json
{
  "clients": [
    {
      "device": {
        "language": "en-GB",
        "name": "PS5",
        "updatedDateTime": "2024-08-25T05:49:20.245Z",
        "wakeupEnabledPowerModes": [
          "networkStandby",
          "mainOnStandby"
        ],
        "enabledFeatures": []
      },
      "duid": "0000000example",
      "platform": "PS5",
      "deviceProperties": [],
      "systemData": {
        "icons": {
          "titles": [
            {
              "comment": "Horizon Forbidden West",
              "conceptId": "10000886",
              "contentPriority": "prior",
              "contentType": "cloudGame",
              "displayLocationSpace": "game",
              "lastAccessDateTime": "2024-08-23T10:50:29.172Z",
              "npTitleId": "PPSA01521_00",
              "psPlatform": "PS5",
              "titleId": "PPSA01521"
            },
            {
              "comment": "Resident Evil: The Darkside Chronicles",
              "contentPriority": "prior",
              "contentType": "cloudGame",
              "displayLocationSpace": "game",
              "lastAccessDateTime": "2024-08-23T10:50:28.827Z",
              "npTitleId": "NPEB00816_00",
              "psPlatform": "PS3",
              "titleId": "NPEB00816"
            },
            {
              "comment": "Ratchet & Clank™: Tools of Destruction",
              "contentPriority": "prior",
              "contentType": "cloudGame",
              "displayLocationSpace": "game",
              "lastAccessDateTime": "2024-08-23T10:50:28.807Z",
              "npTitleId": "NPEA00452_00",
              "psPlatform": "PS3",
              "titleId": "NPEA00452"
            },
            {
              "comment": "Assassin's Creed® IV Black Flag",
              "conceptId": "200089",
              "contentPriority": "prior",
              "contentType": "cloudGame",
              "displayLocationSpace": "game",
              "lastAccessDateTime": "2024-08-23T10:50:26.070Z",
              "npTitleId": "CUSA00009_00",
              "psPlatform": "PS4",
              "titleId": "CUSA00009"
            }
          ],
          "updatedDateTime": "2024-08-25T05:49:28.110Z"
        },
        "installedTitles": {
          "titles": [
            {
              "comment": "Rocket League®",
              "conceptId": "203715",
              "contentPriority": "prior",
              "disc": "inapplicable",
              "displayLocationSpace": "game",
              "lastAccessDateTime": "2024-08-24T20:00:55.716Z",
              "npTitleId": "CUSA01433_00",
              "psPlatform": "PS4",
              "size": 34034458624,
              "storage": "mounted",
              "titleId": "CUSA01433",
              "version": "01.00"
            },
            {
              "comment": "Diablo III: Reaper of Souls – Ultimate Evil Edition",
              "conceptId": "200914",
              "contentPriority": "prior",
              "disc": "unavailable",
              "displayLocationSpace": "game",
              "lastAccessDateTime": "2022-08-20T10:30:25.000Z",
              "npTitleId": "CUSA00433_00",
              "psPlatform": "PS4",
              "size": 52076470272,
              "storage": "mounted",
              "titleId": "CUSA00433",
              "version": "01.00"
            },
            {
              "comment": "Call of Duty®",
              "conceptId": "10001130",
              "contentPriority": "prior",
              "disc": "inapplicable",
              "displayLocationSpace": "game",
              "lastAccessDateTime": "2024-08-25T16:30:11.216Z",
              "npTitleId": "PPSA07950_00",
              "psPlatform": "PS5",
              "size": 155862171648,
              "storage": "m2Extended",
              "titleId": "PPSA07950",
              "version": "01.050.000"
            },
            {
              "comment": "Diablo IV",
              "conceptId": "231761",
              "contentPriority": "prior",
              "disc": "inapplicable",
              "displayLocationSpace": "game",
              "lastAccessDateTime": "2024-08-25T09:26:43.429Z",
              "npTitleId": "PPSA08595_00",
              "psPlatform": "PS5",
              "size": 91380973568,
              "storage": "m2Extended",
              "titleId": "PPSA08595",
              "version": "01.054.000"
            },
            {
              "comment": "ASTRO's PLAYROOM",
              "conceptId": "10000229",
              "contentPriority": "prior",
              "disc": "inapplicable",
              "displayLocationSpace": "game",
              "lastAccessDateTime": "2024-08-23T10:51:01.419Z",
              "npTitleId": "PPSA01325_00",
              "psPlatform": "PS5",
              "size": 11672813568,
              "storage": "embedded",
              "titleId": "PPSA01325",
              "version": "01.902.000"
            },
            {
              "comment": "YouTube",
              "conceptId": "205453",
              "contentPriority": "prior",
              "disc": "inapplicable",
              "displayLocationSpace": "media",
              "lastAccessDateTime": "2024-08-23T10:50:29.238Z",
              "npTitleId": "PPSA01651_00",
              "psPlatform": "PS5",
              "size": 472842240,
              "storage": "embedded",
              "titleId": "PPSA01651",
              "version": "01.000.007"
            }
            ],
          "updatedDateTime": "2024-08-25T21:04:56.229Z"
        },
        "storage": {
          "embedded": {
            "freeSize": 57282658304,
            "totalSize": 667176337408
          },
          "m2Extended": {
            "freeSize": 870432178176,
            "totalSize": 4000760987648
          },
          "mounted": {
            "freeSize": 1240872239104,
            "totalSize": 3673362489344
          },
          "updatedDateTime": "2024-08-25T20:47:50.956Z"
        }
      },
      "updatedDateTime": "2024-08-19T19:14:44.272Z",
      "version": "7"
    }
  ]
}
```
Executing this example using Powershell - see [Querying the API](/APIv2?id=powershell-7)
```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.com/api/cloudAssistedNavigation/v2/users/me/clients?includeFields=device,systemData&platform=PS5" -Headers @{"Accept-Language"="en-gb"} -Authentication Bearer -Token $token | ConvertTo-Json -Depth 10
```