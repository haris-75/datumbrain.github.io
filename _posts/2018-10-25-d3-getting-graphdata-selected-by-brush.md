---
layout: post
title: D3.js - Getting Data Under Brush Selection
author: Nauman Zafar Chaudhry
authorURL: https://github.com/naumanzchaudhry
date: "2018-10-25 16:30:00"
---

<img src="/assets/d3js-icon.svg" style="margin-left:40%;width:150px;"/>

[D3.js](https://d3js.org/) is a powerful JavaScript library to make your information visualization journey a piece of cake (if you like cakes. Infact, I just ate some a couple hours ago). It has an active community(talking about D3 and not cakes) as well in case you get stuck somewhere.

#### Agenda
This article covers getting the data under a brush selection on a canvas using D3 V4.

#### Pre-requisites

The article presumes, you have a basic understanding of JavaScript, JSON and HTML and know how to include a library (like d3.js) and use it.

#### GraphJSON

Whereever in the code below you see a `graph`. It is a `GraphJSON` object. You can learn more about the format of this object [GraphJSON Reference](https://github.com/GraphAlchemist/GraphJSON)

#### What's with this brush?

Brush is actually a cross hair type UI which lets you drag and select an area on a canvas (something like you might have seen while using MS Paint application).

#### Let's dive in

In D3 V4, you can create a brush like this.

1. #####  Creating a Brush
```javascript
// creates a brush.
let brush = d3.brush()
        .extent([[0, 0], [width, height]]);
```

    There are two things to note in above code snippet.
   `d3.brush()` - This is a function `d3` provides for creating a brush.
   `extent()` - extent defines the area this brush can cover. For this case it covers the whole canvas area provided width & height are calculated width & height of the canvas container.

2. ##### Brush end event

    So we want the data. Right? Let's plug some logic when brush ends selecting the canvas area.

    ```javascript
    let svg = $("#svg-container").append("svg").attr("viewBox", "0 0 " + width + " " + height);

    svg.append("g")
      .attr('class', 'brush')
      .call(brush);

    brush.on("end", brushend);
  ```
  Our point of interest in this is `brush.on("end", brushend)` call. This is where we tell d3 that when our brush ends brushing(selecting the area) call the method `brushend`.

3. ##### Let's write brushend method

    Our `brushend` method is going to do a few things.

    1.  Getting the selection of the d3 brush using `d3.event.selection`.

    2.  If we map a canvas to a co-ordinate system we can easily see that If we get the x1, y1, x2, y2 co-ordinates of a 2D space we can get the data under it.

    ```javascript
    function brushend()
    {
      var e = brush.extent().call();

      // get the selection co-ordinates.
      // d3.event.selection is an object [ [x1, y1], [x2,y2] ]
      let selectionCordinates = d3.event.selection;

      // boundary co-ordinates to select nodes from.
      let targetX1 = selectionCordinates[0][0];
      let targetY1 = selectionCordinates[0][1];
      let targetX2 = selectionCordinates[1][0];
      let targetY2 = selectionCordinates[1][1];

      let filteredNodes = graph.nodes.filter(function(n){
        return n.x >= targetX1 && n.x <= targetX2 && n.y >= targetY1 && n.y <=targetY2;
      });
      // Use filtered nodes wherever you want.
    }
    ```

    `d3.event.selection` actually is an array  `[[x1,y1],[x2,y2]]`. So we simply get the boundaries of that selection in `targetX1`, `targetX2`, `targetY1`, `targetY2`.

    Every node in our `GraphJSON` has `x` and `y` co-ordinates which is managed for us by `D3.js`

    `filteredNodes` is just getting a filtered nodes where passed node has `x` and `y` co-ordinates  within our boundaries defined by `target*` variables.
