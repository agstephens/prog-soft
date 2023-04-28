# Working with MongoDB in a python client

```
import pymongo
dbclient = pymongo.MongoClient(IP, PORT_NUMBER)
db = dbclient['phoenix_db']
collection = db.jobs
collection.find()
items = list(collection.find())

print("-------------------------")
print(f"Number of items: {len(items)}")
print("Keys are:")
print("\n\t".join(items[0].keys()))

key = "userid"
print([item[key] for item in items if item[key]][:10])

record = [item for item in items if item[key] == "009e2cfe-330c-11eb-bebb-005056ab528e"][0]
for key, value in record.items():
    print(key, "=", value)

print("\n\n--------------------")
collection = db.users
collection.find()
items = list(collection.find())

key = "login_id"
record = [item for item in items if item[key] == "astephen"][0]
for key, value in record.items():
    print(key, "=", value)


print("Relationship is:\ndb.users.identifier == db.jobs.userid")
print("db.jobs.response also includes:")
print("""
process_id = SubsetCRUTimeSeries
title = SubsetCRUTimeSeries
abstract = Extract a subset from the CRU Time Series data
status_location = http://ceda-wps-ui.ceda.ac.uk/outputs/flamingo/e18579c8-d039-11eb-952f-00505685b1bd.xml
created = 2021-06-18 14:34:17.179000
tags = ['dev', 'async']
caption = None
status = ProcessFailed
""")
```
