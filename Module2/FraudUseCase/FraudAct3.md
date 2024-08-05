# Fraud Act 3

## Use Case 1
Return account and transaction information based on the following input values

- Account Number
- Minimum transactions value
- Start Date
- End Date


Source Account : 7040610000090

Receiving Accounts 

7040610000091
7040610000092
7040610000093
7040610000094
7040610000095
7040610000116
7040610000117

#### Transaction details

~~~
MATCH (p1:Person)-[h1:HOLDS]->(a1:Account)-[s:SENDS]->(t:Transaction)-[r:RECEIVES]->(a2:Account)<-[:HOLDS]-(p2:Person)
WHERE a1.ExternalAccountNumber = toInteger($neodash_account_number)
AND t.Amount >= toInteger($neodash_minimum_transaction_amount)
AND t.CreateDateTime >= datetime({year:$neodash_mindate.year, month:$neodash_mindate.month, day:$neodash_mindate.day})
AND t.CreateDateTime <= datetime({year:$neodash_maxdate.year, month:$neodash_maxdate.month, day:$neodash_maxdate.day})
RETURN toString(a1.ExternalAccountNumber) As SendingAccount, 
p1.FullName As Sender, 
date(t.CreateDateTime) as TransactionDate,
t.Amount AS Amount, 
p2.FullName as Receiver, 
toString(a2.ExternalAccountNumber) as ReceivingAccount
ORDER BY Amount ASC
~~~

