---
layout: post
title: Java/Scala Currency (Package)
author: Fahad Siddiqui
authorUrl: https://github.com/fahadsiddiqui
date: "2019-11-22 15:23:31"
---

It's simple. Let's dive into the code.

```scala
import java.util.Currency
import scala.collection.JavaConverters.asScalaSetConverter

val currencies = Currency.getAvailableCurrencies.asScala

case class CurrencyInfo(code: String, displayName: String, 
  defaultFractionDigits: Long, numericCode: Long, symbol: String)

val currencyMap = currencies.map { 
  c => 
    c.getCurrencyCode -> 
      CurrencyInfo(
        c.getCurrencyCode, 
        c.getDisplayName, 
        c.getDefaultFractionDigits, 
        c.getNumericCode, 
        c.getSymbol
      )
}.toMap

// which gives you the map as shown in following Scala shell output
/*
scala> val currencyMap = currencies.map { 
  c => 
    c.getCurrencyCode -> 
      CurrencyInfo(
        c.getCurrencyCode, 
        c.getDisplayName, 
        c.getDefaultFractionDigits, 
        c.getNumericCode, 
        c.getSymbol
      )
}.toMap

currencyMap: scala.collection.immutable.Map[String,CurrencyInfo] = 
Map(OMR -> CurrencyInfo(OMR,Omani Rial,3,512,OMR), 
BBD -> CurrencyInfo(BBD,Barbadian Dollar,2,52,BBD), PLN -> 
CurrencyInfo(PLN,Polish Zloty,2,985,PLN), CUC -> 
CurrencyInfo(CUC,Cuban Convertible Peso,2,931,CUC), SRG -> 
CurrencyInfo(SRG,Surinamese Guilder,2,740,SRG), 
SVC -> CurrencyInfo(SVC,Salvadoran Col?n,2,222,SVC), BMD -> 
CurrencyInfo(BMD,Bermudan Dollar,2,60,BMD), 
TJS -> CurrencyInfo(TJS,Tajikistani Somoni,2,972,TJS), TND -> 
CurrencyInfo(TND,Tunisian Dinar,3,788,TND), 
GNF -> CurrencyInfo(GNF,Guinean Franc,0,324,GNF), SDG -> 
CurrencyInfo(SDG,Sudanese Pound,2,938,SDG), 
TMM -> CurrencyInfo(TMM,Turkmenistani Manat (1993-2009),2,795,TMM), 
XBB -> CurrencyInfo(XBB,European 
Monetary Unit,-1,956,XBB),...
*/
```

Thanks!

Stay tuned for more Scala/Java stuff!
