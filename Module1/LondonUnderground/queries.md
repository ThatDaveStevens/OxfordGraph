# London Underground Sample Queries

MATCH (start:Station {name: "Paddington"})
MATCH (end:Station {name: "Southwark"})
MATCH path = allShortestPaths((start)-[*..99]-(end))
RETURN path
