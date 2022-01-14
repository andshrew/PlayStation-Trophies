# PlayStation Trophies API v2

With the release of the PlayStation 5 Sony has implemented a revised API for retrieving details of the trophies an account has earned.

This revision of the API is the only way to retrieve trophies for PS5 titles. It is fully backwards compatible and capable for retrieving trophy data associated with tiles on legacy platforms (ie. PS3, PS4 and PS Vita).

## Overview
* Accessing the API requires authentication with a PSN account.
* The information returned will be in the context of the authenticating PSN account.
* You can return the details of another PSN account providing the authenticating PSN account has permission to view the other users trophy list (as utilised by the Compare with User function of the app).

## Notable Changes from API v1
* PS5 titles can now (optionally) track your progress towards earning a trophy, and this progress is available in the new API.
* The v1 API enabled you to retrieve trophy earn rate statistics for any title. In the new API this is more restrictive in that this information is only returned when you request the trophy earned status for an account. If the account you are querying has not played the title then no trophy information is returned. Therefore as it currently stands statistics for PS5 titles can only be retrieved by querying an account which has played the title (and allowed their trophies to sync) at least once.

#   API Documentation

The base URL for accessing this API is: https://m.np.playstation.net/api/trophy

There are three core API endpoints which you will need to make a request to in order to be able to retrieve the trophy details of a user. 

1. A request to retrieve the list of titles (games) for which the user has earned trophies.
2. A request to retrieve the trophies which belong to the title.
3. A request to retrieve the trophies which have been earned for the title.

Successful requests to the API will return a JSON response. These URLs, the inputs they require and the response they return are detailed below.

?> The base URL is actually https://**{platform}**.**{environment}**.playstation.net/api/trophy. `{platform}` can currently be `m` or `ps5`. This will likely also include `web` once https://my.playstation.com has been updated to use these APIs. The `np` environment is the production PSN network and will be the only one accessible via normal PSN accounts.

**Other Examples**<br>
https://ps5.np.playstation.net/api/trophy<br>

## 1. Retrieve the Trophy Titles for a User

    https://m.np.playstation.net/api/trophy/v1/users/{accountId}/trophyTitles

Requests to this URL will retrieve a list of the titles associated with an account, and a summary of trophies earned from them. 

The numeric `accountId` can be that of any PSN account for which the authenticating account has permissions to view the trophy list. When querying the titles associated with the authenticating account the numeric `accountId` can be substituted with `me`.

Included in the information returned is the titles unique `npCommunicationId`. This is required to make use of the subsequent URLs for requesting more specific detail about a titles trophies.

The results are presented in order of the `lastUpdatedDateTime` for the title, so the first result will be the title for which a trophy was recently earned _(or synced for the first time in the case of a game with 0% progress)_.

### Input Parameters

| Parameter | Type | Example Value | Description | Required |
| --- | --- | --- | --- | --- |
| accountId | String | `me`<br>`12340..` | The account whos trophy list is being accessed<br>Use `me` for the authenticating account | Yes
| limit | Numeric<br>**Min** 1<br>**Max** Unknown<br>**Default** ??? | `20` | Limit the number of titles returned | No |
| offset | Numeric<br>**Min** 0<br>**Max** `totalItemCount` - 1<br>**Default** 0 | `20` | Returns title data from this result onwards | No |


### Output JSON Response

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- | 
| trophyTitles | [JSON object](#trophy-titles-trophyTitles-json-objects) | | Individual object for each title returned
| totalItemCount | Numeric | `300` | The total number of trophy titles for this account
| nextOffset | Numeric | `20` | See [Support for Pagination](#support-for-pagination)
| previousOffset | Numeric | `299` | See [Support for Pagination](#support-for-pagination)

##### trophyTitles JSON objects <!-- {docsify-ignore} --> :id=trophy-titles-trophyTitles-json-objects

| Attribute | Type | Example Response | Description |
| --- | --- |--- | --- |
| npServiceName | String | `trophy`<br>`trophy2` | `trophy` for PS3, PS4, or PS Vita platforms<br>`trophy2` for the PS5 platform
| npCommunicationId | String | `NPWR20188_00` | Unique ID of the title; later required for requesting detailed trophy information for this title
| trophySetVersion | String | `01.40` | The current version of the trophy set
| trophyTitleName | String | `ASTRO’s PLAYROOM` | Title name
| trophyTitleDetail | String | `RESIDENT EVIL 5 Trophy Set` | Title description<br>**PS3, PS4 and PS Vita titles only**
| trophyTitleIconUrl | String | `https://...` | URL of the icon for the title
| trophyTitlePlatform | String | `PS5`<br>`PS4` | The platform this title belongs to. Some games have trophy sets which are shared between multiple platforms (ie. PS4,PSVITA). The platforms will be comma separated.
| hasTrophyGroups | Boolean | `true` | True if the title has multiple groups of trophies (eg. DLC trophies which are separate from the main trophy list)
| definedTrophies | [JSON object](#trophy-titles-definedTrophies-json-objects) | | Number of trophies for the title by type
| progress | Numeric | 100 | Percentage of trophies earned for the title
| earnedTrophies | [JSON object](#trophy-titles-earnedTrophies-json-objects) | | Number of trophies for the title which have been earned by type
| hiddenFlag | Boolean | `false` | Title has been hidden on the accounts trophy list<br>**Authenticating account only<br>Title will not be returned if it has been hidden on another account**
| lastUpdatedDateTime | Date (UTC) | `2020-11-21T10:45:19Z` | Date most recent trophy earned for the title

##### definedTrophies JSON objects <!-- {docsify-ignore} --> :id=trophy-titles-definedTrophies-json-objects

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- | 
| bronze | Numeric | `27` | Total bronze trophies from all trophy groups
| silver | Numeric | `13` | Total silver trophies from all trophy groups
| gold | Numeric | `5` | Total gold trophies from all trophy groups
| platinum | Numeric | `1` | Total platinum trophies from all trophy groups

##### earnedTrophies JSON objects <!-- {docsify-ignore} --> :id=trophy-titles-earnedTrophies-json-objects

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- | 
| bronze | Numeric | `27` | Total bronze trophies earned from all trophy groups
| silver | Numeric | `13` | Total silver trophies earned from all trophy groups
| gold | Numeric | `5` | Total gold trophies earned from all trophy groups
| platinum | Numeric | `1` | Total platinum trophies earned from all trophy groups

### Example URLs and Responses

**Example 1 - Retrieve all trophy titles for the authenticating account**

    https://m.np.playstation.net/api/trophy/v1/users/me/trophyTitles

```json
{
  "trophyTitles": [
    {
      "npServiceName": "trophy2",
      "npCommunicationId": "NPWR21575_00",
      "trophySetVersion": "01.07",
      "trophyTitleName": "Destiny 2",
      "trophyTitleIconUrl": "https://psnobj.prod.dl.playstation.net/psnobj/NPWR21575_00/45976ee9-344e-423d-940d-5e42fc82e5b8.png",
      "trophyTitlePlatform": "PS5",
      "hasTrophyGroups": true,
      "definedTrophies": {
        "bronze": 8,
        "silver": 4,
        "gold": 11,
        "platinum": 1
      },
      "progress": 53,
      "earnedTrophies": {
        "bronze": 2,
        "silver": 3,
        "gold": 6,
        "platinum": 0
      },
      "hiddenFlag": false,
      "lastUpdatedDateTime": "2021-03-17T21:58:18Z"
    },
    {
      "npServiceName": "trophy2",
      "npCommunicationId": "NPWR22596_00",
      "trophySetVersion": "01.00",
      "trophyTitleName": "Crash Bandicoot 4: It's About Time",
      "trophyTitleIconUrl": "https://psnobj.prod.dl.playstation.net/psnobj/NPWR22596_00/aeb3ccd0-9346-4845-8a9e-871cf7a7d9d8.png",
      "trophyTitlePlatform": "PS5",
      "hasTrophyGroups": false,
      "definedTrophies": {
        "bronze": 40,
        "silver": 9,
        "gold": 2,
        "platinum": 1
      },
      "progress": 27,
      "earnedTrophies": {
        "bronze": 19,
        "silver": 0,
        "gold": 0,
        "platinum": 0
      },
      "hiddenFlag": false,
      "lastUpdatedDateTime": "2021-03-17T18:05:41Z"
    },
<#-- truncated --#>
    }
  ],
  "totalItemCount": 300
}
```
Executing this example using Powershell - see [Querying the API](#powershell-7)
```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.net/api/trophy/v1/users/me/trophyTitles" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```
**Example 2 - Retrieve up to 10 titles starting at title 50 for the authenticating account**

    https://m.np.playstation.net/api/trophy/v1/users/me/trophyTitles?limit=10&offset=50

```json
{
  "trophyTitles": [
    {
      "npServiceName": "trophy",
      "npCommunicationId": "NPWR18910_00",
      "trophySetVersion": "01.04",
      "trophyTitleName": "Two Point Hospital",
      "trophyTitleDetail": "Two Point Hospital",
      "trophyTitleIconUrl": "https://image.api.playstation.com/trophy/np/NPWR18910_00_00D60FA9AEAADEBBC7C2EB26DB1E2122A86B887436/CBE975BD1527E46830FE821D4CA194D32955C7BE.PNG",
      "trophyTitlePlatform": "PS4",
      "hasTrophyGroups": true,
      "definedTrophies": {
        "bronze": 23,
        "silver": 15,
        "gold": 7,
        "platinum": 1
      },
      "progress": 1,
      "earnedTrophies": {
        "bronze": 1,
        "silver": 0,
        "gold": 0,
        "platinum": 0
      },
      "hiddenFlag": false,
      "lastUpdatedDateTime": "2020-08-18T11:34:58Z"
    },
    <#-- truncated --#>
    }
  ],
  "nextOffset": 60,
  "previousOffset": 49,
  "totalItemCount": 300
}
```
Executing this example using Powershell - see [Querying the API](#powershell-7)
```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.net/api/trophy/v1/users/me/trophyTitles?limit=10&offset=50" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```

## 2. Retrieve the Trophies for a Title

    https://m.np.playstation.net/api/trophy/v1/npCommunicationIds/{npCommunicationId}/trophyGroups/{trophyGroupId}/trophies

A request to this URL will retrieve the individual trophy detail of a single - or all - trophy groups for a title. A title can have multiple groups of trophies (a `default` group which all titles have, and additional groups named `001` incrementing for each additional group). To retrieve trophies from all groups within a title (ie. the full trophy set) then `trophyGroupId` should be set to `all`.

!> When the title platform is PS3, PS4 or PS Vita you __**must**__ specify the `npServiceName` parameter as `trophy`

### Input Parameters

| Parameter | Type | Example Value | Description | Required |
| --- | --- | --- | --- | --- |
| npCommunicationId | String | `NPWR20188_00` | Unique ID of the title | Yes
| trophyGroupId | String | `all`<br>`default`<br>`001` | `all` to return all trophies for the title, otherwise restrict results to a specific trophy group | Yes
| npServiceName | String | `trophy`<br>`trophy2` | `trophy` for PS3, PS4, or PS Vita platforms<br>`trophy2` for the PS5 platform | No, unless title platform is PS3, PS4 or PS Vita then **must** be `trophy`
| limit | Numeric<br>**Min** 1<br>**Max** Unknown<br>**Default** ??? | `20` | Limit the number of trophies returned | No |
| offset | Numeric<br>**Min** 0<br>**Max** `totalItemCount` - 1<br>**Default** 0 | `20` | Returns trophy data from this result onwards | No |

### Output JSON Response

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- |
| trophySetVersion | String | `01.40` | The current version of the trophy set
| hasTrophyGroups | Boolean | `true` | True if this title has additional trophy groups
| trophies | [JSON object](#title-trophies-trophies-json-objects) | | Individual object for each trophy
| totalItemCount | Numeric | `46` | Total trophies in the group (or total trophies for the title if `all` specified)
| nextOffset | Numeric | `30` | See [Support for Pagination](#support-for-pagination)
| previousOffset | Numeric | `19` | See [Support for Pagination](#support-for-pagination)

##### trophies JSON objects <!-- {docsify-ignore} --> :id=title-trophies-trophies-json-objects

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- | 
| trophyId | Numeric<br>**Min** 0  | `0`| Unique ID for this trophy (unique within the title and not just the group)
| trophyHidden | Boolean | `false` | True if this is a secret trophy (ie. further details are not displayed by default unless earned)
| trophyType | String | `bronze`<br>`silver`<br>`gold`<br>`platinum` | Type of the trophy
| trophyName | String | `You've Only Done Everything` | Name of the trophy
| trophyDetail | String | `Found all trophies in ASTRO's PLAYROOM. See you in our next adventure!` | Description of the trophy
| trophyIconUrl | String | `https://...` | URL for the graphic associated with the trophy
| trophyGroupId | String | `default`<br>`001` | ID of the trophy group this trophy belongs to
| trophyProgressTargetValue | String | `10` | If the trophy tracks progress towards unlock this is the total required<br>**PS5 titles only<br>Only returned if trophy tracks progress**
| trophyRewardName | String | `Profile Avatar` | Name of the reward earning the trophy grants<br>**PS5 titles only<br>Only returned if the trophy has a reward associated with it**
| trophyRewardImageUrl | String | `trophyRewardImageUrl` | URL for the graphic associated with the reward<br>**PS5 titles only<br>Only returned if the trophy has a reward associated with it**

### Example URLs and Responses

**Example 1 - Retrieve all trophies for PS5 title ASTRO’s PLAYROOM**

    https://m.np.playstation.net/api/trophy/v1/npCommunicationIds/NPWR20188_00/trophyGroups/all/trophies

```json
{
  "trophySetVersion": "01.40",
  "hasTrophyGroups": true,
  "trophies": [
    {
      "trophyId": 0,
      "trophyHidden": false,
      "trophyType": "platinum",
      "trophyName": "You've Only Done Everything",
      "trophyDetail": "Found all trophies in ASTRO's PLAYROOM. See you in our next adventure!",
      "trophyIconUrl": "https://psnobj.prod.dl.playstation.net/psnobj/NPWR20188_00/4e1f7ad7-5720-4b03-96a4-ecf8abb17ed8.png",
      "trophyGroupId": "default"
    },
<#-- truncated --#>
    {
      "trophyId": 45,
      "trophyHidden": true,
      "trophyType": "bronze",
      "trophyName": "Gravity Daze!",
      "trophyDetail": "Made the CPU Chip punch the glass sphere, sending at least 10 Bots flying.",
      "trophyIconUrl": "https://psnobj.prod.dl.playstation.net/psnobj/NPWR20188_00/ed83d6d9-2708-4465-8f0f-6e3db7ae9665.png",
      "trophyGroupId": "001"
    }
  ],
  "totalItemCount": 46
}
```
Executing this example using Powershell - see [Querying the API](#powershell-7)
```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.net/api/trophy/v1/npCommunicationIds/NPWR20188_00/trophyGroups/all/trophies" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```

**Example 2 - Retrieve all trophies for PS4 title RESIDENT EVIL 5**

    https://m.np.playstation.net/api/trophy/v1/npCommunicationIds/NPWR10600_00/trophyGroups/all/trophies?npServiceName=trophy

```json
{
  "trophySetVersion": "01.01",
  "hasTrophyGroups": true,
  "trophies": [
    {
      "trophyId": 0,
      "trophyHidden": false,
      "trophyType": "platinum",
      "trophyName": "RESIDENT EVIL 5 Platinum Trophy",
      "trophyDetail": "Congratulations! You've overcome all your fears in RESIDENT EVIL 5!",
      "trophyIconUrl": "https://image.api.playstation.com/trophy/np/NPWR10600_00_00A0CF3468337FB7E2039182A26608B0545DE39292/2B4F7ECA102EDB19D18DA17C594DBC50348ECEAF.PNG",
      "trophyGroupId": "default"
    },
<#-- truncated --#>
    {
      "trophyId": 70,
      "trophyHidden": false,
      "trophyType": "bronze",
      "trophyName": "Shoot the Messenger",
      "trophyDetail": "Defeat 3 Agitator Majini in one playthrough of \"Desperate Escape.\"",
      "trophyIconUrl": "https://image.api.playstation.com/trophy/np/NPWR10600_00_00A0CF3468337FB7E2039182A26608B0545DE39292/DB4F19B53DF8C9303728EA28F5A992EF8B3392FD.PNG",
      "trophyGroupId": "003"
    }
  ],
  "totalItemCount": 71
}
```
Executing this example using Powershell - see [Querying the API](#powershell-7)
```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.net/api/trophy/v1/npCommunicationIds/NPWR10600_00/trophyGroups/all/trophies?npServiceName=trophy" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```

**Bonus Example - Extract from a PS5 title with a trophy which tracks unlock progress (NPWR19950_00 - Sackboy: A Big Adventure)**

```json
{
      "trophyId": 35,
      "trophyHidden": false,
      "trophyType": "silver",
      "trophyName": "Naturalist",
      "trophyDetail": "Find all of Gerald's secret spots.",
      "trophyIconUrl": "https://psnobj.prod.dl.playstation.net/psnobj/NPWR19950_00/ee1bc4b9-98d7-46c6-a3ea-1e6b2b258a2a.png",
      "trophyGroupId": "default",
      "trophyProgressTargetValue": "9"
    },
```

**Bonus Example 2 - Extract from a PS5 title with a trophy that grants a reward on unlock (NPWR22792_00 - Destruction AllStars)**

```json
{
      "trophyId": 28,
      "trophyHidden": false,
      "trophyType": "gold",
      "trophyName": "Checking out the competition",
      "trophyDetail": "Win at least 1 online match with each AllStar",
      "trophyIconUrl": "https://psnobj.prod.dl.playstation.net/psnobj/NPWR22792_00/908c536b-c8d4-4591-bd62-b330ccdb20ef.png",
      "trophyGroupId": "default",
      "trophyProgressTargetValue": "16",
      "trophyRewardName": "Profile Avatar",
      "trophyRewardImageUrl": "https://psnobj.prod.dl.playstation.net/psnobj/NPWR22792_00/486de18c-2da3-46e7-8229-4006adb8c28b.png"
}
```

## 3. Retrieve Trophies earned for a Title

    https://m.np.playstation.net/api/trophy/v1/users/{accountId}/npCommunicationIds/{npCommunicationId}/trophyGroups/{trophyGroupId}/trophies

A request to this URL will retrieve the earned status of trophies for a user from either a single - or all - trophy groups in a title. A title can have multiple groups of trophies (a `default` group which all titles have, and additional groups named `001` incrementing for each additional group). To retrieve trophies from all groups within a title (ie. the full trophy set) then `trophyGroupId` should be set to `all`.

The numeric `accountId` can be that of any PSN account for which the authenticating account has permissions to view the trophy list. When querying the titles associated with the authenticating account the numeric `accountId` can be substituted with `me`.

This endpoint returns the earned status of the trophy only and no additional descriptive metadata (ie. trophy name, trophy description). See [Retrieve the Trophies for a Title](#_2-retrieve-the-trophies-for-a-title) to obtain this information.

!> When the title platform is PS3, PS4 or PS Vita you __**must**__ specify the `npServiceName` parameter as `trophy`

!> If you attempt to query a title which the user does not have associated with their account (ie. the title has not been launched and allowed to sync at least once) then a _Resource not found_ error will be returned.

### Input Parameters

| Parameter | Type | Example Value | Description | Required |
| --- | --- | --- | --- | --- |
| accountId | String | `me`<br>`12340..` | The account whos trophy list is being accessed<br>Use `me` for the authenticating account | Yes
| npCommunicationId | String | `NPWR20188_00` | Unique ID of the title | Yes
| trophyGroupId | String | `all`<br>`default`<br>`001` | `all` to return all trophies for the title, otherwise restrict results to a specific trophy group | Yes
| npServiceName | String | `trophy`<br>`trophy2` | `trophy` for PS3, PS4, or PS Vita platforms<br>`trophy2` for the PS5 platform | No, unless title platform is PS3, PS4 or PS Vita then **must** be `trophy`
| limit | Numeric<br>**Min** 1<br>**Max** Unknown<br>**Default** ??? | `20` | Limit the number of trophies returned | No |
| offset | Numeric<br>**Min** 0<br>**Max** `totalItemCount` - 1<br>**Default** 0 | `20` | Returns trophy data from this result onwards | No |

### Output JSON Response

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- |
| trophySetVersion | String | `01.40` | The current version of the trophy set
| hasTrophyGroups | Boolean | `true` | True if this title has additional trophy groups
| lastUpdatedDateTime | Date (UTC) | `2020-11-21T10:45:19Z` | Date most recent trophy earned for the title
| trophies | [JSON object](#earned-trophies-trophies-json-objects) | | Individual object for each trophy
| rarestTrophies | [JSON object](#earned-trophies-raresttrophies-json-objects) | | Individual object for each trophy<br>Returns the trophy where `earned` is `true` with the lowest `trophyEarnedRate`.<br>**Returns nothing if no trophies are earned**
| totalItemCount | Numeric | `46` | Total trophies in the group (or total trophies for the title if `all` specified)
| nextOffset | Numeric | `30` | See [Support for Pagination](#support-for-pagination)
| previousOffset | Numeric | `19` | See [Support for Pagination](#support-for-pagination)

##### trophies JSON objects <!-- {docsify-ignore} --> :id=earned-trophies-trophies-json-objects

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- | 
| trophyId | Numeric<br>**Min** 0  | `0`| Unique ID for this trophy (unique within the title and not just the group)
| trophyHidden | Boolean | `false` | True if this is a secret trophy (ie. further details are not displayed by default unless earned)
| earned | Boolean | `true` | True if this trophy has been earned
| progress | String | `73` | If the trophy tracks progress towards unlock this is number of steps currently completed (ie. 73/300)<br>**PS5 titles only<br>Only returned if the trophy tracks progress and `earned` is `false`**
| progressRate | Numeric | `24` | If the trophy tracks progress towards unlock this is the current percentage complete<br>**PS5 titles only<br>Only returned if the trophy tracks progress and `earned` is `false`**
| progressedDateTime | Date (UTC) | `2020-12-03T22:09:33Z` | If the trophy tracks progress towards unlock, and some progress has been made, then this returns the date progress was last updated.<br>**PS5 titles only<br>Only returned if the trophy tracks progress, some progress has been made, and `earned` is `false`**
| earnedDateTime | Date (UTC) | `2020-11-20T23:53:13Z` | Date trophy was earned<br>**Only returned if `earned` is `true`**
| trophyType | String | `bronze`<br>`silver`<br>`gold`<br>`platinum` | Type of the trophy
| trophyRare | Numeric | `0`<br>`1` | Rarity of the trophy<br>`0` Ultra Rare<br>`1` Very Rare<br>`2` Rare<br>`3` Common
| trophyEarnedRate | String | `8.0` | Percentage of all users who have earned the trophy

##### rarestTrophies JSON objects <!-- {docsify-ignore} --> :id=earned-trophies-raresttrophies-json-objects

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- | 
| trophyId | Numeric<br>**Min** 0  | `0`| Unique ID for this trophy (unique within the title and not just the group)
| trophyHidden | Boolean | `false` | True if this is a secret trophy (ie. further details are not displayed by default unless earned)
| earned | Boolean | `true` | True if this trophy has been earned
| earnedDateTime | Date (UTC) | `2020-11-20T23:53:13Z` | Date trophy was earned
| trophyType | String | `bronze`<br>`silver`<br>`gold`<br>`platinum` | Type of the trophy
| trophyRare | Numeric | `0`<br>`1` | Rarity of the trophy<br>`0` Ultra Rare<br>`1` Very Rare<br>`2` Rare<br>`3` Common
| trophyEarnedRate | String | `8.0` | Percentage of all users who have earned the trophy

### Example URLs and Responses

**Example 1 - Retrieve all trophies earned by the authenticating account for PS5 title ASTRO’s PLAYROOM**

    https://m.np.playstation.net/api/trophy/v1/users/me/npCommunicationIds/NPWR20188_00/trophyGroups/all/trophies

```json
{
  "trophySetVersion": "01.40",
  "hasTrophyGroups": true,
  "lastUpdatedDateTime": "2020-11-21T10:45:19Z",
  "trophies": [
    {
      "trophyId": 0,
      "trophyHidden": false,
      "earned": true,
      "earnedDateTime": "2020-11-20T23:53:13Z",
      "trophyType": "platinum",
      "trophyRare": 1,
      "trophyEarnedRate": "8.0"
    },
    <#-- truncated --#>
    {
      "trophyId": 45,
      "trophyHidden": true,
      "earned": true,
      "earnedDateTime": "2020-11-21T10:45:19Z",
      "trophyType": "bronze",
      "trophyRare": 1,
      "trophyEarnedRate": "12.8"
    }
  ],
  "rarestTrophies": [
    {
      "trophyId": 44,
      "trophyHidden": true,
      "earned": true,
      "earnedDateTime": "2020-11-20T15:21:21Z",
      "trophyType": "gold",
      "trophyRare": 1,
      "trophyEarnedRate": "6.1"
    }
  ],
  "totalItemCount": 46
}
```
Executing this example using Powershell - see [Querying the API](#powershell-7)
```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.net/api/trophy/v1/users/me/npCommunicationIds/NPWR20188_00/trophyGroups/all/trophies" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```

**Example 2 - Retrieve all trophies earned by another PSN account with accountId _0000000000000000000_ limited to the default trophy group of PS4 title RESIDENT EVIL 5**
    
    https://m.np.playstation.net/api/trophy/v1/users/0000000000000000000/npCommunicationIds/NPWR10600_00/trophyGroups/default/trophies?npServiceName=trophy

```json
{
  "trophySetVersion": "01.01",
  "hasTrophyGroups": true,
  "lastUpdatedDateTime": "2017-02-09T22:14:52Z",
  "trophies": [
    {
      "trophyId": 0,
      "trophyHidden": false,
      "earned": true,
      "earnedDateTime": "2017-02-09T22:12:14Z",
      "trophyType": "platinum",
      "trophyRare": 0,
      "trophyEarnedRate": "3.7"
    },
    <#-- truncated --#>
    ],
  "rarestTrophies": [
    {
      "trophyId": 0,
      "trophyHidden": false,
      "earned": true,
      "earnedDateTime": "2017-02-09T22:12:14Z",
      "trophyType": "platinum",
      "trophyRare": 0,
      "trophyEarnedRate": "3.7"
    }
  ],
  "totalItemCount": 51
}
```
Executing this example using Powershell - see [Querying the API](#powershell-7)
```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.net/api/trophy/v1/users/0000000000000000000/npCommunicationIds/NPWR10600_00/trophyGroups/default/trophies?npServiceName=trophy" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```

**Bonus Example - Extract from a PS5 title with a trophy which tracks unlock progress (NPWR19950_00 - Sackboy: A Big Adventure**)

```json
{
      "trophyId": 35,
      "trophyHidden": false,
      "earned": false,
      "progress": "2",
      "progressRate": 22,
      "progressedDateTime": "2020-11-27T16:17:23Z",
      "trophyType": "silver",
      "trophyRare": 1,
      "trophyEarnedRate": "5.4"
    },
```

## Support for Language

By default the API will provide responses using the preferred language of the authenticating account.

This can be overridden by including an [Accept-Language](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Language) header in the request.

An example using Powershell to retrieve all trophies for PS5 title ASTRO’s PLAYROOM in German - see [Querying the API](#powershell-7)

```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.net/api/trophy/v1/npCommunicationIds/NPWR20188_00/trophyGroups/all/trophies" -Authentication Bearer -Token $token -Headers @{"Accept-Language"="de-de"} | ConvertTo-Json -Depth 3
```

```json
{
  "trophySetVersion": "01.40",
  "hasTrophyGroups": true,
  "trophies": [
    {
      "trophyId": 0,
      "trophyHidden": false,
      "trophyType": "platinum",
      "trophyName": "Du hast wirklich alles erledigt",
      "trophyDetail": "Alle Trophäen in ASTRO's PLAYROOM gefunden. Wir sehen uns im nächsten Abenteuer!",
      "trophyIconUrl": "https://psnobj.prod.dl.playstation.net/psnobj/NPWR20188_00/4e1f7ad7-5720-4b03-96a4-ecf8abb17ed8.png",
      "trophyGroupId": "default"
    },
<#-- truncated --#>
}
```
## Support for Pagination

By default the `limit` of results returned is either very high (ie. 1000+), or such that it simply returns all records. In either case this means that in the majority of cases one query will be able to return all available records. There may be times when it is preferable to control how much data is being returned at once, and to enable this many of the endpoints in this API have support for pagination.

If you specify the `limit` and `offset` parameters in your requests then you can control the number of results returned, and in turn the API will also begin including `nextOffset` and `previousOffset` in the response as appropriate. With this information you can then build your next request to allow you to "page" forward or backwards through the available data.

As an example when accessing the trophy titles for a user which has 300 titles where the request parameter `limit` is 10 and `offset` is 50 the response would include:

| Response | Value
| --- | --- |
| nextOffset | 60
| previousOffset | 49

`nextOffset` tells us that to display the next 10 titles then the next request should use `limit` 10 and `offset` 60.

`previousOffset` in this instance tells us the offset for the record immediately prior to the offset we specified in the request. Arguably this should have returned 40 as that would then provide the information needed to "page" 10 items backwards (ie. the next request would be `limit` 10 and `offset` 40)

!> `nextOffset` will not be returned once you have reached the last page of data.

!> If your request includes an `offset` which exceeds the number of items which could be retuned then `previousOffset` is returned stating the offset of the last item.

## Additional Endpoints

### Trophy Profile Summary

    https://m.np.playstation.net/api/trophy/v1/users/{accountId}/trophySummary

A request to this URL will retrieve an overall summary of the number of trophies earned for a user broken down by type, as well as their current overall trophy level, progress towards the next level and which tier their current level falls in to. The tiers are based on the [level changes introduced in 2020](https://andshrew.github.io/PlayStation-Trophies/images/psn-trophy-tiers.png).

The numeric `accountId` can be that of any PSN account for which the authenticating account has permissions to view the trophy list. When querying the titles associated with the authenticating account the numeric `accountId` can be substituted with `me`.

| Tier | Grade | `trophyLevel` Ranges |
| --- | --- | --- |
| 1 | Bronze | 1 - 99 |
| 2 | Bronze | 100 - 199 |
| 3 | Bronze | 200 - 299 |
| 4 | Silver | 300 - 399 |
| 5 | Silver | 400 - 499 |
| 6 | Silver | 500 - 599 |
| 7 | Gold | 600 - 699 |
| 8 | Gold | 700 - 799 |
| 9 | Gold | 800 - 998 |
| 10 | Platinum | 999 |

#### Input Parameters <!-- {docsify-ignore} -->

| Parameter | Type | Example Value | Description | Required |
| --- | --- | --- | --- | --- |
| accountId | String | `me`<br>`12340..` | The account whos trophy list is being accessed<br>Use `me` for the authenticating account | Yes

#### Output JSON Response <!-- {docsify-ignore} -->

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- |
| accountId | String | `12340..` | The ID of the account being accessed
| trophyLevel | Numeric | `403` | The overall trophy level
| progress | Numeric | `67` | Percentage process towards the next trophy level
| tier | Numeric | `5` | The tier this trophy level is in
| earnedTrophies | [JSON object](#overall-summary-earnedtrophies-json-objects) | | Number of trophies which have been earned by type

#### earnedTrophies JSON objects <!-- {docsify-ignore} --> :id=overall-summary-earnedtrophies-json-objects

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- |
| bronze | Numeric | `5225` | Total bronze trophies earned
| silver | Numeric | `1116` | Total silver trophies earned
| gold | Numeric | `355` | Total gold trophies earned
| platinum | Numeric | `37` | Total platinum trophies earned

#### Example URLs and Responses <!-- {docsify-ignore} -->

**Example 1 - Summary of trophies earned by the authenticating account**

    https://m.np.playstation.net/api/trophy/v1/users/me/trophySummary

```json
{
  "accountId": "0000000000000000000",
  "trophyLevel": 403,
  "progress": 67,
  "tier": 5,
  "earnedTrophies": {
    "bronze": 5225,
    "silver": 1116,
    "gold": 355,
    "platinum": 37
  }
}
```
Executing this example using Powershell - see [Querying the API](#powershell-7)
```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.net/api/trophy/v1/users/me/trophySummary" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```

### Title Trophy Groups

    https://m.np.playstation.net/api/trophy/v1/npCommunicationIds/{npCommunicationId}/trophyGroups

A title may have additional groups of trophies. This is most commonly seen in games which have expansions where additional trophies are added.

You can make a request to this URL for a specific title - using the unique `npCommunicationId` for the title - and in response will receive a summary of all of the trophy groups associated with the title. This also includes a summary of the number of trophies for the title broken down by group and type (gold, silver etc.)

!> When the title platform is PS3, PS4 or PS Vita you __**must**__ specify the `npServiceName` parameter as `trophy`

#### Input Parameters <!-- {docsify-ignore} -->

| Parameter | Type | Example Value | Description | Required |
| --- | --- | --- | --- | --- |
| npCommunicationId | String | `NPWR10600_00` | Unique ID of the title | Yes
| npServiceName | String | `trophy`<br>`trophy2` | `trophy` for PS3, PS4, or PS Vita platforms<br>`trophy2` for the PS5 platform | No, unless title platform is PS3, PS4 or PS Vita then **must** be `trophy`

#### Output JSON Response <!-- {docsify-ignore} -->

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- |
| trophySetVersion | String | `01.40` | The current version of the trophy set
| trophyTitleName | String | `ASTRO’s PLAYROOM` | Title name
| trophyTitleDetail | String | `RESIDENT EVIL 5 Trophy Set` | Title description<br>**PS3, PS4 and PS Vita titles only**
| trophyTitleIconUrl | String | `https://...` | URL of the icon for the trophy title
| trophyTitlePlatfrom | String | `PS5` | The platform this title belongs to. Some games have trophy sets which are shared between multiple platforms (ie. PS4,PSVITA). The platforms will be comma separated.
| definedTrophies | [JSON object](#trophy-groups-definedtrophies-json-objects) | | Total number of trophies for the title by type
| trophyGroups | [JSON object](#trophy-groups-trophygroups-json-objects) | | Individual object for each trophy group returned

#### definedTrophies JSON object <!-- {docsify-ignore} --> :id=trophy-groups-definedtrophies-json-objects

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- | 
| bronze | Numeric | `27` | Total bronze trophies from all trophy groups
| silver | Numeric | `13` | Total silver trophies from all trophy groups
| gold | Numeric | `5` | Total gold trophies from all trophy groups
| platinum | Numeric | `1` | Total platinum trophies from all trophy groups

#### trophyGroups JSON objects <!-- {docsify-ignore} --> :id=trophy-groups-trophygroups-json-objects

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- | 
| trophyGroupId | String | `default`<br>`001` | ID for the trophy group (all titles have default, additional groups are 001 incrementing)
| trophyGroupName | String | `ASTRO’s PLAYROOM` | Trophy group name
| trophyGroupDetail | String | `RESIDENT EVIL 5 Trophy Set` | Trophy group description<br>**PS3, PS4 and PS Vita titles only**
| trophyGroupIconUrl | String | `https://...` | URL of the icon for the trophy group
| definedTrophies | [JSON object](#trophy-groups-definedtrophies2-json-objects) | | Number of trophies for the trophy group by type

#### definedTrophies JSON object <!-- {docsify-ignore} --> :id=trophy-groups-definedtrophies2-json-objects

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- | 
| bronze | Numeric | `26` | Total bronze trophies in the trophy group
| silver | Numeric | `13` | Total silver trophies in the trophy group
| gold | Numeric | `3` | Total gold trophies in the trophy group
| platinum | Numeric | `1` | Total platinum trophies in the trophy group

#### Example URLs and Responses <!-- {docsify-ignore} -->

**Example 1 - Retrieve trophy groups for PS5 title ASTRO’s PLAYROOM**

    https://m.np.playstation.net/api/trophy/v1/npCommunicationIds/NPWR20188_00/trophyGroups

```json
{
  "trophySetVersion": "01.40",
  "trophyTitleName": "ASTRO’s PLAYROOM",
  "trophyTitleIconUrl": "https://psnobj.prod.dl.playstation.net/psnobj/NPWR20188_00/035a02db-e64f-4572-8653-4a3db37fe2f6.png",
  "trophyTitlePlatform": "PS5",
  "definedTrophies": {
    "bronze": 27,
    "silver": 13,
    "gold": 5,
    "platinum": 1
  },
  "trophyGroups": [
    {
      "trophyGroupId": "default",
      "trophyGroupName": "ASTRO’s PLAYROOM",
      "trophyGroupIconUrl": "https://psnobj.prod.dl.playstation.net/psnobj/NPWR20188_00/035a02db-e64f-4572-8653-4a3db37fe2f6.png",
      "definedTrophies": {
        "bronze": 26,
        "silver": 13,
        "gold": 3,
        "platinum": 1
      }
    },
    {
      "trophyGroupId": "001",
      "trophyGroupName": "ASTRO’s PLAYROOM [ADD-ON]",
      "trophyGroupIconUrl": "https://psnobj.prod.dl.playstation.net/psnobj/NPWR20188_00/07628edd-c7e7-4762-9409-edcece03a12d.png",
      "definedTrophies": {
        "bronze": 1,
        "silver": 0,
        "gold": 2,
        "platinum": 0
      }
    }
  ]
}
```
Executing this example using Powershell - see [Querying the API](#powershell-7)
```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.net/api/trophy/v1/npCommunicationIds/NPWR20188_00/trophyGroups" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```
### Summary of Trophies Earned by Trophy Group

    https://m.np.playstation.net/api/trophy/v1/users/{accountId}/npCommunicationIds/{npCommunicationId}/trophyGroups

A request to this URL will retrieve a summary of the trophies earned for a user broken down by trophy group within a title. A title can have multiple groups of trophies (a `default` group which all titles have, and additional groups named `001` incrementing for each additional group).

The numeric `accountId` can be that of any PSN account for which the authenticating account has permissions to view the trophy list. When querying the titles associated with the authenticating account the numeric `accountId` can be substituted with `me`.

This endpoint returns the earned summary of the title and its trophy groups only. No additional descriptive metadata is returned (ie. trophy group name, group description). See [Title Trophy Groups](#title-trophy-groups) to obtain this information.

!> When the title platform is PS3, PS4 or PS Vita you __**must**__ specify the `npServiceName` parameter as `trophy`

!> If you attempt to query a title which the user does not have associated with their account (ie. the title has not been launched and allowed to sync at least once) then a _Resource not found_ error will be returned.

#### Input Parameters <!-- {docsify-ignore} -->

| Parameter | Type | Example Value | Description | Required |
| --- | --- | --- | --- | --- |
| accountId | String | `me`<br>`12340..` | The account whos trophy list is being accessed<br>Use `me` for the authenticating account | Yes
| npCommunicationId | String | `NPWR20188_00` | Unique ID of the title | Yes
| npServiceName | String | `trophy`<br>`trophy2` | `trophy` for PS3, PS4, or PS Vita platforms<br>`trophy2` for the PS5 platform | No, unless title platform is PS3, PS4 or PS Vita then **must** be `trophy`

#### Output JSON Response <!-- {docsify-ignore} -->

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- |
| trophySetVersion | String | `01.40` | The current version of the trophy set
| hiddenFlag | Boolean | `false` | Title has been hidden on the accounts trophy list<br>**Authenticating account only<br>Title will not be returned if it has been hidden on another account**
| progress | Numeric | 100 | Percentage of trophies earned for the title
| earnedTrophies | [JSON object](#earned-summary-earnedtrophies-json-objects) | | Number of trophies for the title which have been earned by type
| trophyGroups | [JSON object](#earned-summary-trophygroups-json-objects) | | Individual object for each trophy group returned
| lastUpdatedDateTime | Date (UTC) | `2020-11-21T10:45:19Z` | Date most recent trophy earned for the title

#### earnedTrophies JSON objects <!-- {docsify-ignore} --> :id=earned-summary-earnedtrophies-json-objects

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- |
| bronze | Numeric | `27` | Total bronze trophies earned from all trophy groups
| silver | Numeric | `13` | Total silver trophies earned from all trophy groups
| gold | Numeric | `5` | Total gold trophies earned from all trophy groups
| platinum | Numeric | `1` | Total platinum trophies earned from all trophy groups

#### trophyGroups JSON objects <!-- {docsify-ignore} --> :id=earned-summary-trophygroups-json-objects

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- | 
| trophyGroupId | String | `default`<br>`001` | ID for the trophy group (all titles have default, additional groups are 001 incrementing)
| progress | Numeric | 100 | Percentage of trophies earned for group
| earnedTrophies | [JSON object](#earned-summary-earnedtrophies1-json-objects) | | Number of trophies for the group which have been earned by type
| lastUpdatedDateTime | Date (UTC) | `2020-11-21T10:45:19Z` | Date most recent trophy earned for the group

#### earnedTrophies JSON objects <!-- {docsify-ignore} --> :id=earned-summary-earnedtrophies1-json-objects

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- | 
| bronze | Numeric | `27` | Total bronze trophies earned in the trophy group
| silver | Numeric | `13` | Total silver trophies earned in the trophy group
| gold | Numeric | `5` | Total gold trophies earned in the trophy group
| platinum | Numeric | `1` | Total platinum trophies in the trophy group

#### Example URLs and Responses <!-- {docsify-ignore} -->

**Example 1 - Summary of trophies earned by trophy group for the authenticating account in PS5 title ASTRO’s PLAYROOM**

    https://m.np.playstation.net/api/trophy/v1/users/me/npCommunicationIds/NPWR20188_00/trophyGroups

```json
{
  "trophySetVersion": "01.40",
  "hiddenFlag": false,
  "progress": 100,
  "earnedTrophies": {
    "bronze": 27,
    "silver": 13,
    "gold": 5,
    "platinum": 1
  },
  "lastUpdatedDateTime": "2020-11-21T10:45:19Z",
  "trophyGroups": [
    {
      "trophyGroupId": "default",
      "progress": 100,
      "earnedTrophies": {
        "bronze": 26,
        "silver": 13,
        "gold": 3,
        "platinum": 1
      },
      "lastUpdatedDateTime": "2020-11-20T23:53:13Z"
    },
    {
      "trophyGroupId": "001",
      "progress": 100,
      "earnedTrophies": {
        "bronze": 1,
        "silver": 0,
        "gold": 2,
        "platinum": 0
      },
      "lastUpdatedDateTime": "2020-11-21T10:45:19Z"
    }
  ]
}
```
Executing this example using Powershell - see [Querying the API](#powershell-7)
```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.net/api/trophy/v1/users/me/npCommunicationIds/NPWR20188_00/trophyGroups" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```

**Example 2 - Summary of trophies earned by trophy group for another PSN account with accountId _0000000000000000000_ in PS5 title RESIDENT EVIL 5**

    https://m.np.playstation.net/api/trophy/v1/users/0000000000000000000/npCommunicationIds/NPWR10600_00/trophyGroups?npServiceName=trophy

```json
{
  "trophySetVersion": "01.01",
  "hiddenFlag": false,
  "progress": 74,
  "earnedTrophies": {
    "bronze": 40,
    "silver": 11,
    "gold": 1,
    "platinum": 1
  },
  "lastUpdatedDateTime": "2017-02-09T22:14:52Z",
  "trophyGroups": [
    {
      "trophyGroupId": "default",
      "progress": 100,
      "earnedTrophies": {
        "bronze": 38,
        "silver": 11,
        "gold": 1,
        "platinum": 1
      },
      "lastUpdatedDateTime": "2017-02-09T22:12:14Z"
    },
    {
      "trophyGroupId": "001",
      "progress": 0,
      "earnedTrophies": {
        "bronze": 0,
        "silver": 0,
        "gold": 0,
        "platinum": 0
      }
    },
    {
      "trophyGroupId": "002",
      "progress": 16,
      "earnedTrophies": {
        "bronze": 1,
        "silver": 0,
        "gold": 0,
        "platinum": 0
      },
      "lastUpdatedDateTime": "2016-07-17T17:44:28Z"
    },
    {
      "trophyGroupId": "003",
      "progress": 16,
      "earnedTrophies": {
        "bronze": 1,
        "silver": 0,
        "gold": 0,
        "platinum": 0
      },
      "lastUpdatedDateTime": "2016-07-17T19:49:44Z"
    }
  ]
}
```
Executing this example using Powershell - see [Querying the API](#powershell-7)
```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.net/api/trophy/v1/users/0000000000000000000/npCommunicationIds/NPWR10600_00/trophyGroups?npServiceName=trophy" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```
### Trophy Title Summary for Specific Title ID

    https://m.np.playstation.net/api/trophy/v1/users/{accountId}/titles/trophyTitles?npTitleIds={titleId}

A request to this URL will retrieve a summary of the trophies earned by a user for specific titles.

The `titleId` can be a single title ID, or it can be a comma seperated list of title IDs (%2C when used in a URL). Every title has has an ID assigned to it with these typically starting "CUSA" for PS4 titles and "PPSA" for PS5 titles.

The numeric `accountId` can be that of any PSN account for which the authenticating account has permissions to view the trophy list. When querying the titles associated with the authenticating account the numeric `accountId` can be substituted with `me`.

This endpoint can be used as a way of linking the `npCommunicationId` of a Trophy Set to a titles `npTitleId`, but as with the other user based endpoints in this version of the API you will only get a useful response back if the account you are querying against has played the title.

!> If you attempt to query a title ID which does not exist then a _Resource not found_ error will be returned.

#### Input Parameters <!-- {docsify-ignore} -->

| Parameter | Type | Example Value | Description | Required |
| --- | --- | --- | --- | --- |
| accountId | String | `me`<br>`12340..` | The account whos trophy list is being accessed<br>Use `me` for the authenticating account | Yes
| npTitleIds | String | `PPSA01284_00`<br>`PPSA01284_00%2CCUSA09171_00` | Unique ID of the title | Yes

#### Output JSON Response <!-- {docsify-ignore} -->

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- |
| titles | [JSON object](#trophy-title-summary-for-specific-title-id-titles-json-objects) | | Individual object for each title returned

#### titles JSON objects <!-- {docsify-ignore} --> :id=trophy-title-summary-for-specific-title-id-titles-json-objects

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- |
| npTitleId | String | `PPSA01284_00` | npTitleId of the title
| trophyTitles | [JSON object](#trophy-title-summary-for-specific-title-id-trophyTitles-json-objects) | | Trophy set associated with the title<br>**This will only be returned if the queried account has played the title (and allowed their trophies to sync) at least once**

#### trophyTitles JSON objects <!-- {docsify-ignore} --> :id=trophy-title-summary-for-specific-title-id-trophyTitles-json-objects

| Attribute | Type | Example Response | Description |
| --- | --- |--- | --- |
| npServiceName | String | `trophy`<br>`trophy2` | `trophy` for PS3, PS4, or PS Vita platforms<br>`trophy2` for the PS5 platform
| npCommunicationId | String | `NPWR20004_00` | Unique ID of the trophy set
| trophyTitleName | String | `Returnal` | Title name
| trophyTitleDetail | String | `RESIDENT EVIL 5 Trophy Set` | Title description<br>**PS3, PS4 and PS Vita titles only**
| trophyTitleIconUrl | String | `https://...` | URL of the icon for the title
| hasTrophyGroups | Boolean | `true` | True if the title has multiple groups of trophies (eg. DLC trophies which are separate from the main trophy list)
| rarestTrophies | [JSON object](#trophy-title-summary-for-specific-title-id-rarestTrophies-json-objects) | | Individual object for each trophy<br>Returns the trophy where `earned` is `true` with the lowest `trophyEarnedRate`.<br>**Returns nothing if no trophies are earned**
| progress | Numeric | 100 | Percentage of trophies earned for the title
| earnedTrophies | [JSON object](#trophy-title-summary-for-specific-title-id-earnedTrophies-json-objects) | | Number of trophies for the title which have been earned by type
| definedTrophies | [JSON object](#trophy-title-summary-for-specific-title-id-definedTrophies-json-objects) | | Number of trophies for the title by type
| lastUpdatedDateTime | Date (UTC) | `2021-06-20T12:46:34Z` | Date most recent trophy earned for the title

#### rarestTrophies JSON objects <!-- {docsify-ignore} --> :id=trophy-title-summary-for-specific-title-id-rarestTrophies-json-objects

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- | 
| trophyId | Numeric<br>**Min** 0  | `0`| Unique ID for this trophy (unique within the title and not just the group)
| trophyHidden | Boolean | `false` | True if this is a secret trophy (ie. further details are not displayed by default unless earned)
| trophyType | String | `bronze`<br>`silver`<br>`gold`<br>`platinum` | Type of the trophy
| trophyName | String | `Helios` | Name of the trophy
| trophyDetail | String | `Collect all trophies` | Description of the trophy
| trophyIconUrl | String | `https://...` | URL for the graphic associated with the trophy
| trophyRare | Numeric | `0`<br>`1` | Rarity of the trophy<br>`0` Ultra Rare<br>`1` Very Rare<br>`2` Rare<br>`3` Common
| trophyEarnedRate | String | `2.9` | Percentage of all users who have earned the trophy
| earned | Boolean | `true` | True if this trophy has been earned
| earnedDateTime | Date (UTC) | `2021-06-20T12:46:33Z` | Date trophy was earned

#### earnedTrophies JSON objects <!-- {docsify-ignore} --> :id=trophy-title-summary-for-specific-title-id-earnedTrophies-json-objects

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- | 
| bronze | Numeric | `27` | Total bronze trophies earned from all trophy groups
| silver | Numeric | `13` | Total silver trophies earned from all trophy groups
| gold | Numeric | `5` | Total gold trophies earned from all trophy groups
| platinum | Numeric | `1` | Total platinum trophies earned from all trophy groups

#### definedTrophies JSON objects <!-- {docsify-ignore} --> :id=trophy-title-summary-for-specific-title-id-definedTrophies-json-objects

| Attribute | Type | Example Value | Description |
| --- | --- |--- | --- | 
| bronze | Numeric | `27` | Total bronze trophies from all trophy groups
| silver | Numeric | `13` | Total silver trophies from all trophy groups
| gold | Numeric | `5` | Total gold trophies from all trophy groups
| platinum | Numeric | `1` | Total platinum trophies from all trophy groups

#### Example URLs and Responses <!-- {docsify-ignore} -->

**Example 1 - Summary of trophy titles associated with title IDs PPSA01284_00 (Returnal, PS5), CUSA09171_00 (RESIDENT EVIL 2, PS4) and PPSA04874_00 (Apex Legends, PS5) for the authenticating account**

    https://m.np.playstation.com/api/trophy/v1/users/me/titles/trophyTitles?npTitleIds=CUSA09171_00%2CPPSA01284_00%2CPPSA04874_00

```json
{
  "titles": [
    {
      "npTitleId": "PPSA01284_00",
      "trophyTitles": [
        {
          "npServiceName": "trophy2",
          "npCommunicationId": "NPWR20004_00",
          "trophyTitleName": "Returnal",
          "trophyTitleIconUrl": "https://psnobj.prod.dl.playstation.net/psnobj/NPWR20004_00/f60fd55f-a01f-4274-a865-d8356dc0fd9c.png",
          "hasTrophyGroups": false,
          "rarestTrophies": [
            {
              "trophyId": 0,
              "trophyHidden": false,
              "trophyType": "platinum",
              "trophyName": "Helios",
              "trophyDetail": "Collect all trophies",
              "trophyIconUrl": "https://psnobj.prod.dl.playstation.net/psnobj/NPWR20004_00/aadf79b0-5119-45a2-b925-4e9b6e4bf4ed.png",
              "trophyRare": 0,
              "trophyEarnedRate": "2.9",
              "earned": true,
              "earnedDateTime": "2021-06-20T12:46:33Z"
            }
          ],
          "progress": 100,
          "earnedTrophies": {
            "bronze": 18,
            "silver": 5,
            "gold": 7,
            "platinum": 1
          },
          "definedTrophies": {
            "bronze": 18,
            "silver": 5,
            "gold": 7,
            "platinum": 1
          },
          "lastUpdatedDateTime": "2021-06-20T12:46:34Z"
        }
      ]
    },
    {
      "npTitleId": "CUSA09171_00",
      "trophyTitles": [
        {
          "npServiceName": "trophy",
          "npCommunicationId": "NPWR15179_00",
          "trophyTitleName": "RESIDENT EVIL 2",
          "trophyTitleDetail": "RESIDENT EVIL 2",
          "trophyTitleIconUrl": "https://image.api.playstation.com/trophy/np/NPWR15179_00_004E1F39C12A2C6264BA2A0546D9234F56889DCC5F/BAFEDE2151A73E1FAFE59B8575EC1CF99F6A75F6.PNG",
          "hasTrophyGroups": true,
          "rarestTrophies": [
            {
              "trophyId": 0,
              "trophyHidden": false,
              "trophyType": "platinum",
              "trophyName": "Raccoon City Native",
              "trophyDetail": "Obtain all trophies.",
              "trophyIconUrl": "https://image.api.playstation.com/trophy/np/NPWR15179_00_004E1F39C12A2C6264BA2A0546D9234F56889DCC5F/F957C0C5B0D2B63EBE1BB072B521E2B5A24D2986.PNG",
              "trophyRare": 0,
              "trophyEarnedRate": "3.4",
              "earned": true,
              "earnedDateTime": "2020-10-18T15:23:07Z"
            }
          ],
          "progress": 95,
          "earnedTrophies": {
            "bronze": 29,
            "silver": 9,
            "gold": 4,
            "platinum": 1
          },
          "definedTrophies": {
            "bronze": 30,
            "silver": 10,
            "gold": 4,
            "platinum": 1
          },
          "lastUpdatedDateTime": "2020-10-18T15:23:11Z"
        }
      ]
    },
    {
      "npTitleId": "PPSA04874_00",
      "trophyTitles": []
    }
  ]
}
```
Executing this example using Powershell - see [Querying the API](#powershell-7)
```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.com/api/trophy/v1/users/me/titles/trophyTitles?npTitleIds=CUSA09171_00%2CPPSA01284_00%2CPPSA04874_00" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```

# Querying the API

## Powershell 7

Powershell includes the [`Invoke-RestMethod`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-restmethod?view=powershell-7.1) cmdlet which enables you to to make a request to a URL and process the response - in this case JSON - into a Powershell object.

First configure the authentication token which you will use. See [obtaining an authentication token](#obtaining-an-authentication-token) for more detail.

Now make a request to one of the API URLs using `Invoke-RestMethod`. This will output the response to the screen, but you could instead store the response in a variable or pipe it elsewhere for further processing.

An example to request the authenticating accounts trophy title list and output the original JSON response. A number of other examples are included in the documentation above.

```powershell
Invoke-RestMethod -Uri "https://m.np.playstation.net/api/trophy/v1/users/me/trophyTitles" -Authentication Bearer -Token $token | ConvertTo-Json -Depth 3
```
## Obtaining an Authentication Token

1. Open a Powershell session and paste in the following function
```powershell
function Get-AuthenticationToken {
  param(
    [Parameter(Mandatory=$true)]
    [string]$npsso
  )

  $url = "https://ca.account.sony.com/api/authz/v3/oauth/authorize?access_type=offline&client_id=ac8d161a-d966-4728-b0ea-ffec22f69edc&redirect_uri=com.playstation.PlayStationApp%3A%2F%2Fredirect&response_type=code&scope=psn%3Amobile.v1%20psn%3Aclientapp"

  try {
    $result = Invoke-WebRequest -Uri $url -Headers @{
      "Cookie"="npsso=$npsso"
    }
    Write-Host "Error: Check npsso"
  }
  catch {
    if ($_.Exception.Response.Headers.Location.Query -like "?code=v3*") {
      $query = [System.Web.HttpUtility]::ParseQueryString($_.Exception.Response.Headers.Location.Query)
    }
    else {  Write-Host "Error: Check npsso" }
  }

  $body = @{
    code=$query['code']
    redirect_uri="com.playstation.PlayStationApp://redirect"
    grant_type="authorization_code"
    token_format="jwt"
  }

  $contentType = "application/x-www-form-urlencoded"
  $url = "https://ca.account.sony.com/api/authz/v3/oauth/token"

  try {
    $result = Invoke-WebRequest -Method POST -Uri $url -body $body -ContentType $ContentType -Headers @{
      "Authorization"="Basic YWM4ZDE2MWEtZDk2Ni00NzI4LWIwZWEtZmZlYzIyZjY5ZWRjOkRFaXhFcVhYQ2RYZHdqMHY="
    }
    $token = ConvertTo-SecureString ($result.Content | ConvertFrom-Json).access_token -AsPlainText
    if ($token) {
      Write-Host "Authentication Token successfully granted"
      return $token
    }
    else { Write-Host "Error: Unable to obtain Authentication Token" }
  }
  catch { Write-Host "Error: Unable to obtain Authentication Token" }
}
```

2. In your web browser access https://my.playstation.com and log in with a PSN account.
3. In the same browser access https://ca.account.sony.com/api/v1/ssocookie<br>You should see a response with `npsso` followed by a string of letters and numbers. Highlight and copy this.
![test](images/2021-03-20-15-33-08.png)
4. In the Powershell session run the following command, substituting in your npsso token copied in the previous step
```powershell
$token = Get-AuthenticationToken -npsso "Hwl9Vq%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%"
```
![](images/2021-03-20-15-41-57.png)

The `$token` variable is now ready to be used in combination with `Invoke-RestMethod` to authenticate your requests to the API.

This will be valid for ~60 minutes after which you will need to repeat these steps to obtain an updated token.

## API Rate Limit

!> This information requires confirmation and may be inaccurate for this revision of the API

The API has a rate limit which under testing appears to be 300 requests during a 15 minute period.

Once the limit has been reached the API will begin responding with status code 429. The header of this response will contain `X-RateLimit-Next-Available` the value of which is a unix timestamp for when you will next be able to make requests to the API.