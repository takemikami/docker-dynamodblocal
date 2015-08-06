# DynamoDB Local on Docker
DynamoDB Local on Docker, DynamoDB Local docker container.

This container is used for local-pc development.
You can use this container, when you develop an application
used DynamoDB as strage without aws services.

see.
http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Tools.DynamoDBLocal.html

# Getting started

Start docker container.

```
$ docker pull docker pull takemikami/dynamodblocal
$ docker run -d -v /var/log/dynamodblocal:/var/log -v /var/data/dynamodblocal:/var/dynamodblocal -p 18000:8000 -t takemikami/dynamodblocal
```

Execute sample program.

sample.rb

```
require 'aws-sdk-v1'

options = {api_version: '2012-08-10'}
options[:endpoint] = '192.168.59.103'
options[:port] = 18000
options[:use_ssl] = false
dynamo_client = AWS::DynamoDB::Client.new(options)

dynamo_client.create_table({
      table_name: 'test',
      attribute_definitions: [
        { attribute_name: 'id', attribute_type: 'S' },
      ],
      key_schema: [
        { attribute_name: 'id', key_type: 'HASH' },
      ],
      provisioned_throughput: {
        read_capacity_units: 1,
        write_capacity_units: 1,
      }
})

p dynamo_client.describe_table(table_name: 'test')

dynamo_client.delete_table(table_name: 'test')
```

```
$ gem install aws-sdk-v1
$ ruby sample.rb
```

# Build Container

```
$ docker build -t dynamodblocal .
```
