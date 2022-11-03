# iot_aws_ms
Test project

## MicroService Psado code
```

Main Lambda1(1min timeout ,1gb max mem, event - ALB to Lambda) - > SQS 
Lambda handler gets event raw payload (Json or other)
1st function inspects for all data ( expected 6 fields, Null is not an option -> any errored run -> send SNStopic -> Slack alert with Lambda Logs Location - cloudwatch)
2nd function writes to Dedicated FIFO SQS with the name of the customer and uniqidentifiers - Customer-001-1234532131123-eu-west-1 (customer name, queue number , AWS account , region )


Main Lambda2 -> SQS -> RDS
Lambda handler gets SQS event
1st function extracts origin (Geolocation , farm , Uniqidentifier,time stamp ,DeviceType) -> CALL FUNC3 -> writes to Mysql Dedicated Customer DB.table (Customer.HitEntries-> Insert Farm ,UniqIdentifier , TimeStamp , geolocation )
2nd function extracts water polution state ( float ) if X >= 25.0% ->CALL FUNC3 writes to Mysql Dedicated Customer DB.table (Customer.Polution -> TimeStamp , Polution float ) and Then -> CALL FUNC4 Else ->CALL FUNC3
3rd function write to RDS mysql (map db and table with its content - Customer.HitEntries \ Customer.Polutions)
4th function identified water polution -> send SNStopic -> Slack alert with Captured Polution number , Customer Name , Location

```