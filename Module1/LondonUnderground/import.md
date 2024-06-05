~~~
//SETUP THE DATABASE
CREATE CONSTRAINT station_name FOR (station:Station) REQUIRE station.name IS UNIQUE;
CREATE CONSTRAINT line_name FOR (line:Line) REQUIRE line.name IS UNIQUE;
CREATE FULLTEXT INDEX stations FOR (s:Station) ON EACH [s.name];

//CREATE ZONES
CREATE (:Zone {id: toInteger('1')});
CREATE (:Zone {id: toInteger('2')});
CREATE (:Zone {id: toInteger('3')});
CREATE (:Zone {id: toInteger('4')});
CREATE (:Zone {id: toInteger('5')});
CREATE (:Zone {id: toInteger('6')});
CREATE (:Zone {id: toInteger('7')});
CREATE (:Zone {id: toInteger('8')});
CREATE (:Zone {id: toInteger('9')});

// LOAD STATIONS IN ZONE 1
LOAD CSV WITH HEADERS FROM "file:///Stations.csv" AS LINE FIELDTERMINATOR ',' 
WITH LINE WHERE toFloat(LINE.zone) = toInteger(LINE.zone)
MATCH (z:Zone {id: toInteger(LINE.zone)})
MERGE (station:Station {id: toInteger(LINE.id), longitude: toFloat(LINE.longitude), latitude: toFloat(LINE.latitude), name: LINE.name, zone: toInteger(LINE.zone), total_lines: toInteger(LINE.total_lines), rail: toInteger(LINE.rail)})-[:IN_ZONE]->(z);

//LOAD ALL STATIONS
LOAD CSV WITH HEADERS FROM "file:///Stations.csv" AS LINE FIELDTERMINATOR ',' 
WITH LINE, toInteger(LINE.zone) as firstZone, toInteger(LINE.zone) + 1 as secondZone WHERE toFloat(LINE.zone) <> toInteger(LINE.zone)
MATCH (z1:Zone {id: firstZone}), (z2:Zone {id: secondZone})
MERGE (z1)<-[:IN_ZONE]-(station:Station {id: toInteger(LINE.id), longitude: toFloat(LINE.longitude), latitude: toFloat(LINE.latitude), name: LINE.name, zone: toInteger(LINE.zone), total_lines: toInteger(LINE.total_lines), rail: toInteger(LINE.rail)})-[:IN_ZONE]->(z2);

//LOAD ALL LINES
LOAD CSV WITH HEADERS FROM "file:///Lines.csv" AS LINE FIELDTERMINATOR ',' WITH LINE
MERGE (:Line {id: toInteger(LINE.id), name: LINE.name, colour: LINE.colour, rel_name: LINE.rel_name});

//CONNECT STATIONS TO LINES
LOAD CSV WITH HEADERS FROM "file:///Line_Stations.csv" AS LINE FIELDTERMINATOR ',' WITH LINE
MATCH (s1:Station {id: toInteger(LINE.station1)}), (s2:Station {id: toInteger(LINE.station2)}), (line:Line {id: toInteger(LINE.line)})
MERGE (s1)-[:ON]->(line)
MERGE (s2)-[:ON]->(line)
WITH s1, line.rel_name as rlabel, s2
CALL apoc.create.relationship(s1, rlabel,{time:'1'}, s2) YIELD rel
RETURN count(rel);

//DROP THE BUILDING DATA
MATCH p=()-[r:ON]->() delete r;
MATCH p=()-[r:IN_ZONE]->() delete r;
MATCH (n:Zone) detach delete n;
MATCH (n:Line) detach delete n;
~~~