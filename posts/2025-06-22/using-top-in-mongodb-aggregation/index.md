<!--
.. title: Using top in mongodb aggregation
.. slug: using-top-in-mongodb-aggregation
.. date: 2025-06-22 20:59:15 UTC+02:00
.. tags: mongodb,db,aggregation,top
.. category: db
.. link: 
.. description: 
.. type: text
-->

As mongo docs says:
```
MongoDB aggregation stage limits
Each stage can use up to 100 MB of RAM. You will get an error from the database if you exceed this limit. 
```
So if you are having a problem and you need the first (or last) element from a sorted collection, you can use the top operator. 

So instead of sorting:
```
db.Entity.aggregate([
    {
        "$match": {
            "startTime": {
                "$gte": ISODate("2025-04-02T22:00:00Z"),
                "$lt": ISODate("2025-04-03T22:00:00Z")
                }
            }
        },
    {
        "$addFields": {
            "creatorPriority": {
                "$switch": {
                    "branches": [
                        { "case": { "$eq": ["$creator", "ADMIN"] }, "then": 1 }
                        ],
                    "default": 0
                    }
                }
            }
        },
    {
        "$sort": {
            "creatorPriority": -1,
            "createdAt": -1
            }
        },
    {
        "$group": {
            "_id": {
                "deviceId": "$deviceId",
                "startTime": "$startTime"
                },
            "records": { "$push": "$$CURRENT" }
            }
        },
    {
        "$replaceRoot": {
            "newRoot": { "$first": "$records" }
            }
        }
    ])

```

Use top:
```mongodb
db.Entity.aggregate([
    {
        "$match": {
            "startTime": {
                "$gte": ISODate("2025-06-18T22:00:00Z"),
                "$lt": ISODate("2025-06-20T22:00:00Z")
                }
            }
        },
    {
        "$addFields": {
            "creatorPriority": {
                "$switch": {
                    "branches": [
                        { "case": { "$eq": ["$creator", "ADMIN"] }, "then": 1 }
                        ],
                    "default": 0
                }
            }
        }
    },

    {
        "$group": {
            "_id": {
                "entityId": "$entityId",
                "startTime": "$startTime"
                },
            "topRecord": {
                "$top": {
                    "sortBy": { "createdAt": -1 },
                    "output": "$$ROOT"
                    }
                }
            }
        },
    {
        "$replaceRoot": {
            "newRoot": "$topRecord"
            }
        }
    ] , {
    allowDiskUse: false
    })

```