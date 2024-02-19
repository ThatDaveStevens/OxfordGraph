MATCH (p:Person)-[r:HAS_TELEPHONE_HOME]->(id)<-[r2:HAS_TELEPHONE_HOME]-(p2:Person)
RETURN p,r,id,r2,p2

MATCH (a:Address)<-[r3]-(p:Person)-[r:HAS_TELEPHONE_HOME]->(th:TelephoneHome)<-[r2:HAS_TELEPHONE_HOME]-(p2:Person)-[r4]->(a2:Address)
RETURN p,r,th,r2,p2,a,a2,r3,r4

MATCH (p:Person)-[r:HAS_TELEPHONE_HOME|HAS_EMAIL|LIVES_AT]->(id)<-[r2:HAS_TELEPHONE_HOME|HAS_EMAIL|LIVES_AT]-(p2:Person)
RETURN p,r,id,r2,p2

MATCH (p:Person)-[r]->(id)<-[r2]-(p2:Person)
RETURN p,r,id,r2,p2