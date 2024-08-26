## User Played Games (PS4 titles and newer)

      https://m.np.playstation.com/api/gamelist/v2/users/{accountId}/titles

This endpoint retrieves a users played games list, which includes stats such as the number of times the game has been played, the first and most recent dates the game was played, and the total time played (accurate to 1 second). It also returns additional metadata about a game including URLs for media used on the PlayStation Store, as well as all the Title Ids currently associated with the game.  

Games released for PS4 (and newer) are included in the list. By default it is sorted by most recently played.  

The endpoint can query the authenticating account, or it can query another user account (providing that the privacy settings on the other account allow it).  

### Input Parameters

| Parameter | Type | Example Value | Description | Required |
| --- | --- | --- | --- | --- |
| accountId | String | `me`<br>`12340..` | The id of the account to query<br>Use `me` to query the authenticating account | Yes
| categories | String | `ps4_game,ps5_native_game`<br>`pspc_game`<br>`unknown` | Comma separated list of platforms | No
| limit | Numeric<br>**Min** 1<br>**Max** 200<br>**Default** 10 | `10` | Limit the number of titles returned | No
| offset | Numeric<br>**Min** 0<br>**Max** `totalItemCount` - 1<br>**Default** 0 | `20` | Returns title data from this result onwards<br>See [Support for Pagination](/APIv2?id=support-for-pagination) | No |


### Output JSON Response

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- | 
| titles | [JSON object](#user-played-games-titles-json-objects) | | Individual object for each title returned
| totalItemCount | Numeric | `300` | The total number of trophy titles for this account
| nextOffset | Numeric | `20` | See [Support for Pagination](/APIv2?id=support-for-pagination)
| previousOffset | Numeric | `299` | See [Support for Pagination](/APIv2?id=support-for-pagination)

##### titles JSON objects <!-- {docsify-ignore} --> :id=user-played-games-titles-json-objects

| Attribute | Type | Example Response | Description |
| --- | --- |--- | --- |
| titleId | String | `CUSA01433_00` | Id for the specific version of the game played by the user
| name | String | `Rocket League®` | Name of the game
| localizedName | String | `Rocket League®` | Name of the game
| imageUrl | String | `https://iamge...` | URL for the game icon
| localizedImageUrl | String | `https://image...` | URL for the game icon
| category | String | `ps4_game`<br>`ps5_native_game`<br>`pspc_game`<br>`unknown` | Type of game
| service | String | `none(purchased)`<br>`none_purchased`<br>`ps_plus` | Is the game owned outright, or via a service entitlement
| playCount | Numeric | `100` | Number of times the game has been played
| concept | JSON object | | The concept is an single identifier for the various versions of a game<br>This object contains various metadata including the `conceptId` and the various Title Ids for this game
| media | JSON object | | This object contains various URLs for screenshots and other media associated with the game
| firstPlayedDateTime | Date (UTC) | `2015-07-10T19:40:19Z` | Date the game was first played
| lastPlayedDateTime | Date (UTC) | `2024-08-03T19:28:27.12Z` | Date the game was most recently played
| playDuration | [ISO 8601 Duration](https://en.wikipedia.org/wiki/ISO_8601) | `PT228H56M33S` | Time played accurate to 1 second<br>This example reads 228 hours, 56 minutes and 33 seconds

##### Working with Time Durations <!-- {docsify-ignore} -->

When querying the API with PowerShell you can use the following function to convert the ISO 8601 duration string into a TimeSpan object, which will be easier to manipulate.  

```powershell
[Xml.XmlConvert]::ToTimeSpan()
```

An example for `PT228H56M33S`:  
```powershell
[Xml.XmlConvert]::ToTimeSpan("PT228H56M33S")
```

Output:  
```powershell
Days              : 9
Hours             : 12
Minutes           : 56
Seconds           : 33
Milliseconds      : 0
Ticks             : 8241930000000
TotalDays         : 9.53927083333333
TotalHours        : 228.9425
TotalMinutes      : 13736.55
TotalSeconds      : 824193
TotalMilliseconds : 824193000
```

#### Example URLs and Responses <!-- {docsify-ignore} -->

#### Example 1 - Games list for the authenticating account <!-- {docsify-ignore} -->

    https://m.np.playstation.com/api/gamelist/v2/users/me/titles

```json
{
  "titles": [
    {
      "titleId": "PPSA07950_00",
      "name": "Call of Duty®",
      "localizedName": "Call of Duty®",
      "imageUrl": "https://image.api.playstation.com/vulcan/ap/rnd/202406/1421/5811b9a8ab59c7703c3d4f0a60748c029208aed35f28d7f3.png",
      "localizedImageUrl": "https://image.api.playstation.com/vulcan/ap/rnd/202406/1421/5811b9a8ab59c7703c3d4f0a60748c029208aed35f28d7f3.png",
      "category": "ps5_native_game",
      "service": "none(purchased)",
      "playCount": 392,
      "concept": {
        "id": 10001130,
        "titleIds": "CUSA34084_00 PPSA07950_00 PPSA01649_00 PPSA09262_00 CUSA23826_00 CUSA35564_00 CUSA43691_00 CUSA48767_00 CUSA34032_00 PPSA07953_00 CUSA23827_00 CUSA34087_00 CUSA34083_00 CUSA43690_00 CUSA34029_00 PPSA09265_00 CUSA48768_00 CUSA35561_00 CUSA43694_00 CUSA34031_00 CUSA34086_00 CUSA48765_00 PPSA09264_00 CUSA48769_00 CUSA35562_00 CUSA43693_00 PPSA07951_00 CUSA34030_00 CUSA34085_00 CUSA35563_00 CUSA48766_00 PPSA09263_00 CUSA43692_00 PPSA07952_00",
        "name": "Call of Duty®",
        "media": "@{audios=System.Object[]; videos=System.Object[]; images=System.Object[]}",
        "genres": "ACTION",
        "localizedName": "@{defaultLanguage=en-US; metadata=}",
        "country": "GB",
        "language": "en"
      },
      "media": {
        "audios": "",
        "videos": "",
        "images": "              "
      },
      "firstPlayedDateTime": "2022-10-20T20:26:17Z",
      "lastPlayedDateTime": "2024-08-03T20:40:21.78Z",
      "playDuration": "PT340H46M13S"
    },
    {
      "titleId": "CUSA01433_00",
      "name": "Rocket League®",
      "localizedName": "Rocket League®",
      "imageUrl": "https://image.api.playstation.com/vulcan/ap/rnd/202406/1015/b3767253dc1f28bc5213300131216efe3eb351b98f7bc37a.png",
      "localizedImageUrl": "https://image.api.playstation.com/vulcan/ap/rnd/202406/1015/b3767253dc1f28bc5213300131216efe3eb351b98f7bc37a.png",
      "category": "ps4_game",
      "service": "ps_plus",
      "playCount": 685,
      "concept": {
        "id": 203715,
        "titleIds": "CUSA02837_00 CUSA01759_00 CUSA05310_00 CUSA01163_00 CUSA01433_00 CUSA03686_00 CUSA10695_00 CUSA05270_00 CUSA13031_00 CUSA10690_00 CUSA01653_00 CUSA12808_00 CUSA12970_00",
        "name": "Rocket League®",
        "media": "@{audios=System.Object[]; videos=System.Object[]; images=System.Object[]}",
        "genres": "SPORTS RACING ACTION",
        "localizedName": "@{defaultLanguage=en-US; metadata=}",
        "country": "GB",
        "language": "en"
      },
      "media": {
        "audios": "",
        "videos": "",
        "images": "          "
      },
      "firstPlayedDateTime": "2015-07-10T19:40:19Z",
      "lastPlayedDateTime": "2024-08-03T19:28:27.12Z",
      "playDuration": "PT228H56M33S"
    },
    <#-- truncated --#>
    ],
  "nextOffset": 10,
  "previousOffset": 0,
  "totalItemCount": 900
}

```
Executing this example using Powershell - see [Querying the API](/APIv2?id=powershell-7)
```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.com/api/gamelist/v2/users/me/titles" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```

!> Note the Concept and Media attributes are truncated by this command. If you want to capture the full response then increase `ConvertTo-Json -Depth 3` to something like `10`, or omit this command and capture the response into a PowerShell object.

#### Example 2 - Games list for another PSN account with accountId _0000000000000000000_ <!-- {docsify-ignore} -->

    https://m.np.playstation.com/api/gamelist/v2/users/0000000000000000000/titles

Executing this example using Powershell - see [Querying the API](/APIv2?id=powershell-7)
```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.com/api/gamelist/v2/users/0000000000000000000/titles" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```

!> Note the Concept and Media attributes are truncated by this command. If you want to capture the full response then increase `ConvertTo-Json -Depth 3` to something like `10`, or omit this command and capture the response into a PowerShell object.

#### Example 3 - PS5 only games list for the authenticating account <!-- {docsify-ignore} -->

    https://m.np.playstation.com/api/gamelist/v2/users/me/titles?categories=ps5_native_game

Executing this example using Powershell - see [Querying the API](/APIv2?id=powershell-7)
```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.com/api/gamelist/v2/users/me/titles?categories=ps5_native_game" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```

!> Note the Concept and Media attributes are truncated by this command. If you want to capture the full response then increase `ConvertTo-Json -Depth 3` to something like `10`, or omit this command and capture the response into a PowerShell object.