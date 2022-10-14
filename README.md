
# SS modifier

An API for converting your surge configuration.

You can simply insert, replace, or delete some config in your surge profile. All you need to do is writing a modifier config file and update your managed URL.

- [Intro](#Intro)
- [Modifier format](#Modifier)
- [Example](#Example)

## Intro

SS modifier is written in pure Swift (using vapor).

Feel free to open pull request or leave any issues.

## Build

Just clone this repo and open `Package.swift` in `server` folder (using Xcode).

## Send Request

**Get** `https://ss.kaelzs.com/surge/convert`

### Parameters

> `urls` (String or [String], required): Your surge modifier urls.
>
> `name` (String, optional, default to `surge.conf`): The response file name.
>
> `preview` (Bool, optional, default to false): if set to true, the response content type will be set to `text/plain`.
>
> `managed` (Bool, optional, default to true): If set to false, the surge config will not be managed by URL.
>
> `interval` (Bool, optional, default to 3600): the update interval of managed config.
>
> `strict` (Bool, optional, default to false): weather needs a strict update when managed config outdated.

## Modifier

### Group Modifier

There are two kinds of group modifier, `replace` or `modify`, you can declare the type in any surge group, the default type is `replace`, you can specify the type using `#!type $TYPE`.

The replacing modifier will replace the entire group of the current profile, while the modifying modifier will just do some modification to the group of the current profile.

You can also specify the name of the modifier using `#!name $NAME`

Supported Options:

- `#!basedOnResources`: to indicate that the modifier is based on the resource and the modifier will be ignored if the resource fails to load.

- `#!requiredModifiers $NAMEA, $NAMEB`: to indicate that the modifier is based on the specific modifier and the modifier will be ignored if any of the modifiers is ignored.

``` Properties
[General]
#!type replace
#!name general0
#!basedOnResources

[Replica]
#!type modify
#!requiredModifiers general0
```

### Plain line

The plain line is only supported in replacing modifier, you can just write the config as in the surge config. The plain line will be ignored when written in a basic modifier.

``` Properties