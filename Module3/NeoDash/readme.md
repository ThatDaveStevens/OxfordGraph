Example text search values

What is the total value of all transactions?



which person has the most transactions, show the total number of transactions and the total value?


Query to create a Graph View

MATCH (p:Person)-[:HAS_ACCOUNT]->(a:Account)-[:SENDS]->(t:Transaction)
WITH p, COUNT(t) as TransactionCount, SUM(t.Amount) as TransactionValue
ORDER BY TransactionCount DESC
LIMIT 1
WITH p
MATCH (p)-[r:HAS_ACCOUNT]->(a:Account)-[r2:SENDS]->(t:Transaction)
RETURN p,a,t,r,r2

