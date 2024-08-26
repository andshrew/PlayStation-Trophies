## Universal Search

      https://m.np.playstation.com/api/graphql/v1/op?operationName=metGetContextSearchResults

This endpoint can search for user accounts, or it can search the PlayStation Store.  

### Input Parameters

| Parameter | Value 
| --- | --- 
| operationName | `metGetContextSearchResults`
| variables | `{"searchTerm":"username","searchContext":"MobileUniversalSearchSocial","displayTitleLocale":"en-US"}`
| extensions | `{"persistedQuery":{"version":1,"sha256Hash":"ac5fb2b82c4d086ca0d272fba34418ab327a7762dd2cd620e63f175bbc5aff10"}}`

| Property | Parent Parameter | Type | Example Values | Description | Required |
| --- | --- | --- | --- | --- | --- |
| searchTerm | variables | String | `username`<br>`game name` | The username or PS Store content name to search for | Yes |
| searchContext | variables | String | `MobileUniversalSearchSocial`<br>`MobileUniversalSearchGame` | The type of search to perform | Yes |
| displayTitleLocale | variables | String | `en-US` | The language and country code of the store to search | Yes |

#### Additional Headers <!-- {docsify-ignore} -->

Requests to this endpoint must include additional headers in the request.

| Key | Value |
| --- | --- |
| apollographql-client-name | PlayStationApp-Android |
| content-type | application/json |

### Example URLs and Responses

#### Example 1 - Search for user account "username" <!-- {docsify-ignore} -->

    https://m.np.playstation.com/api/graphql/v1/op?operationName=metGetContextSearchResults&variables={"searchTerm":"username","searchContext":"MobileUniversalSearchSocial","displayTitleLocale":"en-US"}&extensions={"persistedQuery":{"version":1,"sha256Hash":"ac5fb2b82c4d086ca0d272fba34418ab327a7762dd2cd620e63f175bbc5aff10"}}

```json
{
  "data": {
    "universalContextSearch": {
      "__typename": "UniversalContextSearchResponse",
      "queryFrequency": {
        "__typename": "QueryFrequency",
        "filterDebounceMs": 1000,
        "searchDebounceMs": 500
      },
      "results": [
        {
          "__typename": "UniversalDomainSearchResponse",
          "domain": "SocialAllAccounts",
          "domainTitle": "Players",
          "next": "eyJTTyI6MTV9",
          "searchResults": [
            {
              "__typename": "SearchResultItem",
              "highlight": {
                "__typename": "PlayerHighlight",
                "firstName": null,
                "lastName": null,
                "middleName": null,
                "onlineId": [
                  "",
                  "Username"
                ],
                "verifiedUserName": null
              },
              "id": "ABCxyz==",
              "result": {
                "__typename": "Player",
                "accountId": "0000000000000000000",
                "avatarUrl": "http://psn-rsc.prod.dl.playstation.net/psn-rsc/avatar/UT0016/CUSA06833_00-AV00000000000001_72E7CFC37BB24D706E60_l.png",
                "displayName": "Username",
                "displayNameHighlighted": [],
                "firstName": null,
                "id": "ABCxyz==",
                "isPsPlus": false,
                "itemType": "SOCIAL",
                "lastName": null,
                "middleName": null,
                "onlineId": "Username",
                "onlineIdHighlighted": [
                  "",
                  "Username"
                ],
                "profilePicUrl": null,
                "relationshipState": null
              },
              "resultOriginFlag": null
            },
            <#-- truncated --#>
          "totalResultCount": 128,
          "zeroState": false
        }
      ]
    }
  }
}
```

Executing this example using Powershell - see [Querying the API](/APIv2?id=powershell-7)
```powershell
Invoke-RestMethod -Uri 'https://m.np.playstation.com/api/graphql/v1/op?operationName=metGetContextSearchResults&variables=
{"searchTerm":"username","searchContext":"MobileUniversalSearchSocial","displayTitleLocale":"en-US"}&extensions=
{"persistedQuery":{"version":1,"sha256Hash":"ac5fb2b82c4d086ca0d272fba34418ab327a7762dd2cd620e63f175bbc5aff10"}}' -Headers @{"apollographql-client-name"="PlayStationApp-Android";"content-type"="application/json"} -Authentication Bearer -Token $token | ConvertTo-Json -Depth 10
```

#### Example 2 - Search for "batman" content on the PS Store <!-- {docsify-ignore} -->

    https://m.np.playstation.com/api/graphql/v1/op?operationName=metGetContextSearchResults&variables={"searchTerm":"batman","searchContext":"MobileUniversalSearchGame","displayTitleLocale":"en-US"}&extensions={"persistedQuery":{"version":1,"sha256Hash":"ac5fb2b82c4d086ca0d272fba34418ab327a7762dd2cd620e63f175bbc5aff10"}}

```json
{
  "data": {
    "universalContextSearch": {
      "__typename": "UniversalContextSearchResponse",
      "queryFrequency": {
        "__typename": "QueryFrequency",
        "filterDebounceMs": 1000,
        "searchDebounceMs": 500
      },
      "results": [
        {
          "__typename": "UniversalDomainSearchResponse",
          "domain": "MobileGames",
          "domainTitle": "Full Games",
          "next": "CA8abQo6YWIzYjAzZDctZTZjZi00YjU3LWI2MzEtYzVjY2Y4MGYyMDIwLThOVG1SeWRKLTItMTcyNDY5NTIwMBIvc2VhcmNoLXJlbGV2YW5jeS1jb25jZXB0LWdhbWUtbWwtbW9kZWwtYmxhY2tleWUiHnNlYXJjaC5ub19leHBlcmltZW50Lm5vbi4wLm5vbioELTk0NA",
          "searchResults": [
            {
              "__typename": "SearchResultItem",
              "highlight": {
                "__typename": "ItemHighlight",
                "name": [
                  "",
                  "BATMAN",
                  "™: ARKHAM KNIGHT"
                ]
              },
              "id": "200472",
              "result": {
                "__typename": "Concept",
                "defaultProduct": {
                  "__typename": "Product",
                  "id": "UP1018-CUSA00133_00-BATMANARKHAMKNHT",
                  "invariantName": "Batman™: Arkham Knight",
                  "itemType": "CONCEPTPRODUCT",
                  "localizedStoreDisplayClassification": "Full Game",
            <#-- truncated --#>
          "totalResultCount": 83,
          "zeroState": false
        }
      ]
    }
  }
}
```

Executing this example using Powershell - see [Querying the API](/APIv2?id=powershell-7)
```powershell
Invoke-RestMethod -Uri 'https://m.np.playstation.com/api/graphql/v1/op?operationName=metGetContextSearchResults&variables=
{"searchTerm":"batman","searchContext":"MobileUniversalSearchGame","displayTitleLocale":"en-US"}&extensions=
{"persistedQuery":{"version":1,"sha256Hash":"ac5fb2b82c4d086ca0d272fba34418ab327a7762dd2cd620e63f175bbc5aff10"}}' -Headers @{"apollographql-client-name"="PlayStationApp-Android";"content-type"="application/json"} -Authentication Bearer -Token $token | ConvertTo-Json -Depth 10
```

## Universal Search Pagination

      https://m.np.playstation.com/api/graphql/v1/op?operationName=metGetDomainSearchResults

This endpoint compliments the main [Universal Search](#universal-search) and allows you to page through additional search results. It requires input parameters returned in the response to that endpoint.    

### Input Parameters

| Parameter | Value 
| --- | --- 
| operationName | `metGetDomainSearchResults`
| variables | `{"searchTerm":"username","searchDomain":"SocialAllAccounts","nextCursor":"eyJTTyI6MTV9","pageSize":20,"pageOffset":40}`
| extensions | `{"persistedQuery":{"version":1,"sha256Hash":"23ece284bf8bdc50bfa30a4d97fd4d733e723beb7a42dff8c1ee883f8461a2e1"}}`

| Property | Parent Parameter | Type | Example Values | Description | Required |
| --- | --- | --- | --- | --- | --- |
| searchTerm | variables | String | `username`<br>`game name` | The username or PS Store content name that was searched for | Yes |
| searchDomain | variables | String | `SocialAllAccounts`<br>`MobileGames` | The type of search that was performed | Yes |
| nextCursor | variables | String | `eyJTTyI6MTV9` | This was returned as part of the [Universal Search](#universal-search) response | Yes |
| pageSize | variables | Numeric | `20` | Number of results to return in the page | Yes |
| pageOffset | variables | Numeric | `40` | Unclear why an offset is required when a cursor position is being supplied with `nextCursor`, but requests from the app do increment this as it pages through results | Yes |


#### Additional Headers <!-- {docsify-ignore} -->

Requests to this endpoint must include additional headers in the request.

| Key | Value |
| --- | --- |
| apollographql-client-name | PlayStationApp-Android |
| content-type | application/json |

### Example URLs and Responses

#### Example 1 - Get an additional page of search results for user account "username" <!-- {docsify-ignore} -->

    https://m.np.playstation.com/api/graphql/v1/op?operationName=metGetDomainSearchResults&variables={"searchTerm":"username","searchDomain":"SocialAllAccounts","nextCursor":"eyJTTyI6MTV9","pageSize":20,"pageOffset":100}&extensions={"persistedQuery":{"version":1,"sha256Hash":"23ece284bf8bdc50bfa30a4d97fd4d733e723beb7a42dff8c1ee883f8461a2e1"}}

```json
{
  "data": {
    "universalDomainSearch": {
      "__typename": "UniversalDomainSearchResponse",
      "domain": "SocialAllAccounts",
      "domainTitle": "Players",
      "next": "eyJTTyI6MzV9",
      "searchResults": [
        {
          "__typename": "SearchResultItem",
          "highlight": {
            "__typename": "PlayerHighlight",
            "firstName": [
              "",
              "username"
            ],
            "lastName": null,
            "middleName": null,
            "onlineId": [
              "",
              "Username",
              ""
            ],
            "verifiedUserName": null
          },
          "id": "ABCxyz==",
          "result": {
            "__typename": "Player",
            "accountId": "0000000000000000000",
            "avatarUrl": "http://static-resource.np.community.playstation.net/avatar/WWS_E/E2100_l.png",
            "displayName": "username",
            "displayNameHighlighted": [
              "",
              "username",
              " ",
              "",
              ""
            ],
            "firstName": "username",
            "id": "ABCxyz==",
            "isPsPlus": false,
            "itemType": "SOCIAL",
            "lastName": null,
            "middleName": null,
            "onlineId": "Username",
            "onlineIdHighlighted": [
              "",
              "Username",
              ""
            ],
            "profilePicUrl": "https://image.api.playstation.com/profile/images/filestore/...",
            "relationshipState": null
          },
          "resultOriginFlag": null
        },
            <#-- truncated --#>
          "totalResultCount": 128,
      "zeroState": false
    }
  }
}
```

Executing this example using Powershell - see [Querying the API](/APIv2?id=powershell-7)
```powershell
Invoke-RestMethod -Uri 'https://m.np.playstation.com/api/graphql/v1/op?operationName=metGetDomainSearchResults&variables={"searchTerm":"username","searchDomain":"SocialAllAccounts","nextCursor":"eyJTTyI6MTV9","pageSize":20,"pageOffset":100}&extensions={"persistedQuery":{"version":1,"sha256Hash":"23ece284bf8bdc50bfa30a4d97fd4d733e723beb7a42dff8c1ee883f8461a2e1"}}' -Headers @{"apollographql-client-name"="PlayStationApp-Android";"content-type"="application/json"} -Authentication Bearer -Token $token | ConvertTo-Json -Depth 10
```

#### Example 2 - Get an additional page of search results for "batman" content on the PS Store <!-- {docsify-ignore} -->

    https://m.np.playstation.com/api/graphql/v1/op?operationName=metGetDomainSearchResults&variables={"searchTerm":"batman","searchDomain":"MobileGames","nextCursor":"CA8abQo6YWIzYjAzZDctZTZjZi00YjU3LWI2MzEtYzVjY2Y4MGYyMDIwLThOVG1SeWRKLTItMTcyNDY5NTIwMBIvc2VhcmNoLXJlbGV2YW5jeS1jb25jZXB0LWdhbWUtbWwtbW9kZWwtYmxhY2tleWUiHnNlYXJjaC5ub19leHBlcmltZW50Lm5vbi4wLm5vbioELTk0NA","pageSize":20,"pageOffset":20}&extensions={"persistedQuery":{"version":1,"sha256Hash":"23ece284bf8bdc50bfa30a4d97fd4d733e723beb7a42dff8c1ee883f8461a2e1"}}

```json
{
  "data": {
    "universalDomainSearch": {
      "__typename": "UniversalDomainSearchResponse",
      "domain": "MobileGames",
      "domainTitle": "Full Games",
      "next": "",
      "searchResults": [
        {
          "__typename": "SearchResultItem",
          "highlight": {
            "__typename": "ItemHighlight",
            "name": null
          },
          "id": "228618",
          "result": {
            "__typename": "Concept",
            "defaultProduct": {
              "__typename": "Product",
              "id": "UP9000-CUSA01623_00-0000GODOFWAR3PS4",
              "invariantName": "God of War III Remastered",
              "itemType": "CONCEPTPRODUCT",
              "localizedStoreDisplayClassification": "Full Game",
            <#-- truncated --#>
          "totalResultCount": 40,
      "zeroState": false
    }
  }
}
```

Executing this example using Powershell - see [Querying the API](/APIv2?id=powershell-7)
```powershell
Invoke-RestMethod -Uri 'https://m.np.playstation.com/api/graphql/v1/op?operationName=metGetDomainSearchResults&variables={"searchTerm":"batman","searchDomain":"MobileGames","nextCursor":"CA8abQo6YWIzYjAzZDctZTZjZi00YjU3LWI2MzEtYzVjY2Y4MGYyMDIwLThOVG1SeWRKLTItMTcyNDY5NTIwMBIvc2VhcmNoLXJlbGV2YW5jeS1jb25jZXB0LWdhbWUtbWwtbW9kZWwtYmxhY2tleWUiHnNlYXJjaC5ub19leHBlcmltZW50Lm5vbi4wLm5vbioELTk0NA","pageSize":20,"pageOffset":20}&extensions={"persistedQuery":{"version":1,"sha256Hash":"23ece284bf8bdc50bfa30a4d97fd4d733e723beb7a42dff8c1ee883f8461a2e1"}}' -Headers @{"apollographql-client-name"="PlayStationApp-Android";"content-type"="application/json"} -Authentication Bearer -Token $token | ConvertTo-Json -Depth 10
```