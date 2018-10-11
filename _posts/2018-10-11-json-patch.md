---
layout: post
title: Updating (Patching) Json Dynamically in Scala
author: Fahad Siddiqui
date: "2018-10-11 15:51:00"
---

Recently I've found a cleaner way to patch (update) your Json while working with Microservices framework `Lagom`.<br>

Thanks to Gnieh Diffson @ [https://github.com/gnieh/diffson](https://github.com/gnieh/diffson) for creating this wonderful library.

## Dependencies

### SBT
```
libraryDependencies += "org.gnieh" %% f"diffson-$LIBRARY" % "3.0.0"
```

Where `LIBRARY` can be

* `play-json`
* `spray-json`
* `circe`

In my case, as I work most of the time with `SBT`

```
libraryDependencies += "org.gnieh" %% "diffson-play-json" % "3.0.0"
```

### Maven
```
<dependency>
  <groupId>org.gnieh</groupId>
  <artifactId>diffson-${json.lib}_${scala.version}</artifactId>
  <version>3.0.0</version>
</dependency>
```

#### Quoting [diffjson](https://github.com/gnieh/diffson)
> "These versions are built for Scala 2.11, 2.12, and 2.13-M3 when the underlying json library already works with 2.13."

## Code

Here are the imports for the code
```
import play.api.libs.json.{JsValue, Json}
```

Here lies the first two `JsValue`s
```
val jsvalue1 = Json.parse(
  """
    |{
    |  "id": 3,
    |  "text": "Hey what's up!"
    |}
  """.stripMargin)

val jsvalue2 = Json.parse(
  """
    |{
    |  "id": 3,
    |  "text": "Revised, hey what's up?"
    |}
  """.stripMargin)
```

To patch the first and update the text (dynamically) by using `gneih`'s `JsonPatch`, we just have to

```
val patch = JsonPatch(jsvalue1, jsvalue2, remember=false)
```

and then later on apply the patch to `jsvalue1`,

```
// patch.apply()
patch(jsvalue1)
```

The new updated `json` is (`JsValue` returned from `patch(jsvalue1)`)

```
{
  "id": 3,
  "text": "Revised, hey what's up?"
}
```

Thanks to [https://github.com/gnieh/diffson](https://github.com/gnieh/diffson)!
