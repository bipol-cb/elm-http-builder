# elm-http-builder

[![ICARE](https://icarebadge.com/ICARE-white.png)](https://icarebadge.com)
[![Build Status](https://travis-ci.org/lukewestby/elm-http-builder.svg?branch=master)](https://travis-ci.org/lukewestby/elm-http-builder)

Chainable functions for building HTTP requests.

**Need help? Join the #http channel in the [Elm Slack](https://elmlang.herokuapp.com)!**


> Thanks to @fredcy, @rileylark, and @etaque for the original discussion of the
  API, and to @knewter for pairing and discussion on the 0.18 upgrade.

## Example

In this example, we expect a successful response to be JSON array of strings,
like:

```json
["hello", "world", "this", "is", "the", "best", "json", "ever"]
```

and an error response might have a body which just includes text, such as the
following for a 404 error:

```json
Not Found.
```

```elm
import Time
import Http
import HttpBuilder exposing (..)
import Json.Decode as Decode
import Json.Encode as Encode


itemsDecoder : Decode.Decoder (List String)
itemsDecoder =
    Decode.list Decode.string


itemEncoder : String -> Encode.Value
itemEncoder item =
    Encode.object
        [ ("item", Encode.string item) ]


handleRequestComplete : Result Http.Error (List String) -> Msg
handleRequestComplete result =
    -- Handle the result


addItem : String -> Cmd Msg
addItem item =
    HttpBuilder.post "http://example.com/api/items"
        |> withJsonBody (itemEncoder item)
        |> withTimeout (10 * Time.second)
        |> withExpect (Http.expectJson itemsDecoder)
        |> withCredentials
        |> send handleRequestComplete
```

## Contributing

 I'm happy to receive any feedback and ideas for about additional features. Any
input and pull requests are very welcome and encouraged. If you'd like to help
or have ideas, get in touch with me at @luke_dot_js on Twitter or @luke in the
elmlang Slack!
