---
layout: post
title: Multiple (Unique) Graph Databases On Same Server (Neo4j)
author: Saad Ali
date: "2018-10-08 09:53:00"
---
> Prerequisites for this article:<br>
>1- General Understanding of Graph Databases<br>
>2- General Understanding of Neo4j<br>

While working on a product at [Datum Brain](https://www.datumbrain.com/) we reached at an interesting situation.

It was an `NLP` related project where we were mapping entities on `nodes` in a `graph database`. We were using `Neo4j` to achieve that.

### Problem
Anyway, we had one instance of `Neo4j` server running and what we wanted to do was to create multiple independent databases on it. Seems simple enough right?

Well, what caused the problem was, if there were two nodes with same data (which is totally possible, like two same values inputted by two different users), `Neo4j` created two nodes and by itself assigned them two different `ids`. Like if we run the below statement twice
```
CREATE (Person:p {title:'Jon Doe'})
```
Neo4j created two nodes like this <br><br>
![](/post-assests/neo4j_bolt.png)
<br><br>
It didn't (obviously) met our business requirements as even though the nodes had same data they might not necessarily have the same relations with other nodes right? And how would we retrieve a certain node and its related nodes if we don't know the `id` assigned to it by `Neo4j`?

### Solution
To tackle this problem, what we did was, we added another property to the node,like this
```
CREATE (Person:p {title:'Jon Doe',id:'1'})
```
That property `id` is unique and is a part of every node belonging to a certain database. Now when we wanted to retrieve a certain database/graph, what we did was just add a condition in `where` clause to retrieve only the nodes belonging to a certain database i.e. having `id` equal to a unique value we assigned to a certain database, something like this
```
MATCH (e: Person)-[]->(l: Location) WHERE Id = '$Id' AND l.Id = '$Id' RETURN (e)-[]->(l);
```
Now even if some nodes had same data they would never have same `Id` thus making or not making it a part of a certain database and giving us power to create multiple independent instances of it in different databases on same server. Cool isn't it?
