## Identify people sending regular payments to the same individual each month

### Sample Datasets
#### Group 1
Small amount, same day every month
- amount £100
- 25th each month


#### Group 2
Similar amount (+- 5%), same day every month

- Amounts £95, 96, 97, 98, 99, 100, 101, 102, 103, 104, 105
- 18th each month

#### Group 3
Similar amount, within 2 days each month

- Amounts £95, 96, 97, 98, 99, 100, 101, 102, 103, 104, 105
- Dates 11th,12th,13th

_Extended transactions files to include act 2 data_


7040610000005,7040610000006,7040610000007,7040610000008,7040610000009,7040610000010





List of sending accounts
7040610000005
7040610000006
7040610000007
7040610000008
7040610000009
7040610000010


Receiving account 
7040610000004




List of sending accounts
7040610000013
7040610000014
7040610000015
7040610000016
7040610000017
7040610000018

receiving account 
7040610000012



List of sending accounts
7040610000021
7040610000022
7040610000023
7040610000024
7040610000025
7040610000026


Receiving account
7040610000020


---

Act 2.1

Find any shared personal information

Fixing the data

005 and 006 share the same personal email
006 and 007 share the same home address
007 and 008 share the same mobile phone number
007 and 009 share the same mobile phone number
005 and 010 share the same work number
009 and 008 share the same home address


Common transactions within the group

~~~
WITH [7040610000005,7040610000006,7040610000007,7040610000008,7040610000009,7040610000010] AS accountList
MATCH path=(p1:Person)-[:HOLDS]->(a1:Account)-[:SENDS|RECEIVES]-(t:Transaction)-[:SENDS|RECEIVES]-(a2:Account)<-[:HOLDS]-(p2:Person)
WHERE a1.ExternalAccountNumber IN accountList
RETURN path
~~~

Shared personal information within the group

~~~
WITH [7040610000005,7040610000006,7040610000007,7040610000008,7040610000009,7040610000010] AS accountList
MATCH path=(a1:Account)<-[:HOLDS]-(p1:Person)-[l1]-(id)-[l2]-(p2:Person)-[:HOLDS]-(a:Account)
WHERE a1.ExternalAccountNumber IN accountList
RETURN path
~~~




