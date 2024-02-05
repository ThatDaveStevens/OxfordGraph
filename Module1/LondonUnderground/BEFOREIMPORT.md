CALL apoc.schema.assert({},{},true) YIELD label, key, unique, action
RETURN label, key, unique, action