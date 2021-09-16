# PlayStation Trophies API <!-- {docsify-ignore} -->

Sony have multiple APIs for retrieving details of the trophies an account has earned, but there is no public documentation for using them. This is an attempt at documenting these APIs.

With the release of the PlayStation 5 Sony implemented a revised API for retrieving details of the trophies an account has earned. This revision of the API is referred to as v2 in this documentation, and it is the only way to retrieve trophies for PS5 titles. It remains fully backwards compatible and capable for retrieving trophy data associated with tiles on legacy platforms (ie. PS3, PS Vita, PS4).

These APIs have been documented by by capturing the requests made by the https://my.playstation.com web site (v1 of the API), and through analysis of the PS App mobile app (v2 of the API).

* [PlayStation Trophies API v1](APIv1.md)
    * Supported platforms PS4, PS3 and PS Vita
* [PlayStation Trophies API v2](APIv2.md)
    * Supported platforms PS5, PS4, PS3 and PS Vita

## Additional Thanks <!-- {docsify-ignore} -->

With thanks to projects [Tustin/psn-php](https://github.com/Tustin/psn-php) and [games-directory/api-psn](https://github.com/games-directory/api-psn) for their work on understanding authentication to https://ca.account.sony.com/api/authz/v3, and to [Ragowit](https://github.com/Ragowit) for their contribution in documenting the APIs.


[View on GitHub andshrew/PlayStation-Trophies](https://github.com/andshrew/PlayStation-Trophies/)