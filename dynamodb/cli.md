# Common command AWS CLI

## List table
```bash
aws dynamodb list-tables
```
```json
{
    "TableNames": [
        "dev-users"
    ]
}
```

## Describe a table
```bash
aws dynamodb describe-table --table-name dev-users
```
```json
{
    "Table": {
        "AttributeDefinitions": [
            {
                "AttributeName": "timestamp",
                "AttributeType": "S"
            },
            {
                "AttributeName": "user_id",
                "AttributeType": "S"
            }
        ],
        "TableName": "dev-users",
        "KeySchema": [
            {
                "AttributeName": "user_id",
                "KeyType": "HASH"
            },
            {
                "AttributeName": "timestamp",
                "KeyType": "RANGE"
            }
        ],
        "TableStatus": "ACTIVE",
        "CreationDateTime": 1626539317.64,
        "ProvisionedThroughput": {
            "NumberOfDecreasesToday": 0,
            "ReadCapacityUnits": 1,
            "WriteCapacityUnits": 1
        },
        "TableSizeBytes": 0,
        "ItemCount": 0,
        "TableArn": "arn:aws:dynamodb:xxx:xxx:table/dev-users",
        "TableId": "a4a374e9-a8a4-4b7a-862a-xxx",
        "LocalSecondaryIndexes": [
            {
                "IndexName": "user_id-timestamp-index",
                "KeySchema": [
                    {
                        "AttributeName": "user_id",
                        "KeyType": "HASH"
                    },
                    {
                        "AttributeName": "timestamp",
                        "KeyType": "RANGE"
                    }
                ],
                "Projection": {
                    "ProjectionType": "ALL"
                },
                "IndexSizeBytes": 0,
                "ItemCount": 0,
                "IndexArn": "arn:aws:dynamodb:xxx:XXX:table/dev-users/index/user_id-timestamp-index"
            }
        ]
    }
}
```

## Create a table
```bash
aws dynamodb create-table --table-name dev-users-test --attribute-definitions AttributeName=user_id,AttributeType=S AttributeName=timestamp,AttributeType=N --key-schema AttributeName=user_id,KeyType=HASH AttributeName=timestamp,KeyType=RANGE --provisioned-throughput ReadCapacityUnits=1,WriteCapacityUnits=1
```
```json
{
    "TableDescription": {
        "AttributeDefinitions": [
            {
                "AttributeName": "timestamp",
                "AttributeType": "N"
            },
            {
                "AttributeName": "user_id",
                "AttributeType": "S"
            }
        ],
        "TableName": "dev-users-test",
        "KeySchema": [
            {
                "AttributeName": "user_id",
                "KeyType": "HASH"
            },
            {
                "AttributeName": "timestamp",
                "KeyType": "RANGE"
            }
        ],
        "TableStatus": "CREATING",
        "CreationDateTime": 1626540130.591,
        "ProvisionedThroughput": {
            "NumberOfDecreasesToday": 0,
            "ReadCapacityUnits": 1,
            "WriteCapacityUnits": 1
        },
        "TableSizeBytes": 0,
        "ItemCount": 0,
        "TableArn": "arn:aws:dynamodb:xxx:xxx:table/dev-users-test",
        "TableId": "dc3fd70b-7c7a-438e-bb70-xxx"
    }
}
```

## Delete a table
```bash
aws dynamodb delete-table --table-name dev-users-test
```
```json
{
    "TableDescription": {
        "TableName": "dev-users-test",
        "TableStatus": "DELETING",
        "ProvisionedThroughput": {
            "NumberOfDecreasesToday": 0,
            "ReadCapacityUnits": 1,
            "WriteCapacityUnits": 1
        },
        "TableSizeBytes": 0,
        "ItemCount": 0,
        "TableArn": "arn:aws:dynamodb:xxx:xxx:table/dev-users-test",
        "TableId": "dc3fd70b-7c7a-438e-bb70-xxx"
    }
}
```

## Put a item
```bash
aws dynamodb put-item --table-name dev-users --item file://item.json
```

## Update a item
```bash
aws dynamodb update-item --table-name dev-users --key file://key.json --update-expression "SET #c = :c" --expression-attribute-names file://attribute-names.json --expression-attribute-values file://attribute-values.json 
```

## Delete a item
```bash
aws dynamodb delete-item --table-name dev-users --key file://key.json
```

## Batch write item
```bash
aws dynamodb batch-write-item --request-items file://items.json
```
```json
{
    "UnprocessedItems": {}
}
```

## Read a item
```bash
aws dynamodb get-item --table-name dev-users --key file://read-key.json
```
```json
{
    "Item": {
        "user_id": {
            "S": "1"
        },
        "username": {
            "S": "thanh"
        },
        "age": {
            "N": "27"
        },
        "timestamp": {
            "S": "1626539236"
        }
    }
}
```

## Execute a query
```bash
aws dynamodb query --table-name dev-users --key-condition-expression "user_id = :uid" --expression-attribute-value file://expression-attribute-values.json
```
```json
{
    "Items": [
        {
            "user_id": {
                "S": "1"
            },
            "username": {
                "S": "thanh"
            },
            "age": {
                "N": "27"
            },
            "timestamp": {
                "S": "1626539236"
            }
        }
    ],
    "Count": 1,
    "ScannedCount": 1,
    "ConsumedCapacity": null
}
```

## Scan table
```bash
aws dynamodb scan --table-name dev-users --filter-expression "username = :uname" --expression-attribute-values file://username.json
```
```json
{
    "Items": [
        {
            "user_id": {
                "S": "1"
            },
            "username": {
                "S": "thanh"
            },
            "age": {
                "N": "27"
            },
            "timestamp": {
                "S": "1626539236"
            }
        }
    ],
    "Count": 1,
    "ScannedCount": 5,
    "ConsumedCapacity": null
}
```