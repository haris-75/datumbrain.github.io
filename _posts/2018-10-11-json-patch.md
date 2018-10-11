---
layout: post
title: Updating (Patching) Json Dynamically in Scala
author: Fahad Siddiqui
date: "2018-10-11 15:51:00"
---

![](/assets/posts/programming.png)

Recently I've found a cleaner way to patch (update) your Json while working with Microservices framework `Lagom`.<br>

Thanks to Gnieh Diffson @ [https://github.com/gnieh/diffson](https://github.com/gnieh/diffson) for creating this wonderful library.

## Dependencies

### SBT
```sbt
libraryDependencies += "org.gnieh" %% f"diffson-$LIBRARY" % "3.0.0"
```

Where `LIBRARY` can be

* `play-json`
* `spray-json`
* `circe`

In my case, as I work most of the time with `SBT`

```sbt
libraryDependencies += "org.gnieh" %% "diffson-play-json" % "3.0.0"
```

### Maven
```xml
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
```scala
import play.api.libs.json.{JsValue, Json}
```

Here lies the first two `JsValue`s
```scala
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

```scala
val patch = JsonDiff.diff(jsvalue1, jsvalue2, remember=false)
```

and then later on apply the patch to `jsvalue1`,

```scala
// patch.apply()
patch(jsvalue1)
```

> Note: You can also convert the `patch` to `JsValue` if you need to serialize it and use somewhere.
> To do that, just `Json.parse(patch.toString)` and you'll get the `JsValue`

The new updated `json` is (`JsValue` returned from `patch(jsvalue1)`)

```json
{
  "id": 3,
  "text": "Revised, hey what's up?"
}
```
