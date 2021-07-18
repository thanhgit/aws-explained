# Use dynamodb with aws-sdk

## Install aws-sdk in nodejs
```bash
npm install aws-sdk --save
```

## Global configure
```javascript
const AWS = require("aws-sdk");
AWS.config.update({ region: 'us-east-1' });
```

- ### Table level
```javascript
const dynamodb = new AWS.DynamoDB();
```

- ### Item level
```javascript
const docClient = new AWS.DynamoDB.DocumentClient();
```

- ### List tables
```javascript
dynamodb.listTables({}, (err, data)=>{
    if(err) {
        console.log(err);
    } else {
        console.log(data);
    }
});
```

- ### Describe a table
```javascript
dynamodb.describeTable({
    TableName: "dev-users"
}, (err, data)=>{
    if(err) {
        console.log(err);
    } else {
        console.log(JSON.stringify(data, null, 2));
    }
});
```

- ### Create a table
```javascript
dynamodb.createTable({
    TableName: "dev-users",
    AttributeDefinitions: [
        {
            AttributeName: "user_id",
            AttributeType: "S"
        },
        {
            AttributeName: "timestamp",
            AttributeType: "N"
        }
    ],
    KeySchema: [
        {
            AttributeName: "user_id",
            KeyType: "HASH"
        },
        {
            AttributeName: "timestamp",
            KeyType: "RANGE"
        }
    ],
    ProvisionedThroughput: {
        ReadCapacityUnits: 1,
        WriteCapacityUnits: 1
    }
}, (err, data)=>{
    if(err) {
        console.log(err);
    } else {
        console.log(JSON.stringify(data, null, 2));
    }
});
```

- ### Update a table with provisioned throughput
```javascript
dynamodb.updateTable({
    TableName: "dev-users",
    ProvisionedThroughput: {
        ReadCapacityUnits: 2,
        WriteCapacityUnits: 1
    }
}, (err, data) => {
    if(err) {
        console.log(err);
    } else {
        console.log(JSON.stringify(data, null, 2));
    }
});
```

- ### Delete a table
```javascript
dynamodb.deleteTable({
    TableName: "dev-users"
}, (err, data) => {
    if(err) {
        console.log(err);
    } else {
        console.log(JSON.stringify(data, null, 2));
    }
});
```
