# London Underground Sample Queries

~~~
MATCH (start:Station {name: "Paddington"})
MATCH (end:Station {name: "Southwark"})
MATCH path = allShortestPaths((start)-[*..99]-(end))
RETURN path
~~~

![image](images/LDNBrowser.png)<br>
_Example results within the Neo4j Browser_

![image](images/LDNBloom.png)<br>
_Example results within the Neo4j Bloom_

