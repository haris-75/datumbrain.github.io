---
layout: post
title: CSV to JSON Using Jackson (Scala/Java)
author: Fahad Siddiqui
date: "2018-09-25 17:02:00"
---

To convert a file containing CSV into JSON format, there are a few classes in `com.fasterxml.jackson` package.

Using maven dependency below

```xml
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.8.2</version>
</dependency>
<dependency>
  <groupId>com.fasterxml.jackson.dataformat</groupId>
  <artifactId>jackson-dataformat-csv</artifactId>
  <version>2.8.2</version>
</dependency>
```

Here is what I tried in Scala

```scala
import com.fasterxml.jackson.dataformat.csv.CsvSchema
import com.fasterxml.jackson.dataformat.csv.CsvMapper
import com.fasterxml.jackson.databind.ObjectMapper
import java.io.File

def csvFileToJson(filePath: String): String = {
  val inputCsvFile = new File(path)

  // if the csv has header, use setUseHeader(true)
  val csvSchema = CsvSchema.builder().setUseHeader(true).build()
  val csvMapper = new CsvMapper()

  // java.util.Map[String, String] identifies they key values type in JSON
  val readAll = csvMapper
    .readerFor(classOf[java.util.Map[String, String]])
    .`with`(csvSchema)
    .readValues(inputCsvFile)
    .readAll()

  val mapper = new ObjectMapper()

  // json return value
  mapper.writerWithDefaultPrettyPrinter().writeValueAsString(readAll)
}
```

This returns JSON as a string. So, if you want to convert that into a case class
in Scala, you can try `play-json`.
