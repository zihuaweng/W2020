* Title:

  SELECT JSON "firstName" FROM ... results in {"\"firstName\"": "Bill"}

* Link: 

  https://issues.apache.org/jira/browse/CASSANDRA-12760

* Description: 

  User Niek Bartholomeus created this issue on 07/Oct/16 reporting the following issue: 

> I'm using Cassandra to store data coming from Spark and intended for being consumed by a javascript front end.
>
> To avoid unnecessary field name mappings I have decided to use mixed case fields in Cassandra. I also happily leave it to Cassandra to jsonify the data (using SELECT JSON ...) so my scala/play web server can send the results from Cassandra straight through to the front end.
>
> I noticed however that all mixed case fields (that were created with quotes as Cassandra demands) end up having a double set of quotes
>
> ```CQL
> create table user(id text PRIMARY KEY, "firstName" text);
> insert into user(id, "firstName") values ('b', 'Bill');
> select json * from user;
> 
> [json]
> --------------------------------------
> {"id": "b", "\"firstName\"": "Bill"}
> ```
>
> Ideally that would be:
>
> ```CQL
> [json]
> --------------------------------------
>  {"id": "b", "firstName": "Bill"}
> ```
>
> I worked around it for now by removing all "\""'s before sending the json to the front end.

The issue was considered a bug at first, but changed to an improvement later. Because Cassandra behaves like this for a reason:

> "The results of SELECT JSON are designed to be usable in an INSERT JSON statement without any modifications, so all of the same rules about non-text map keys and case-sensitive column names apply."

(from https://www.datastax.com/blog/2015/06/whats-new-cassandra-22-json-support)

The creator Niek Bartholomeus believes that 

> as json is intended for communication with the outside world it should ideally not contain any cassandra-specific implementation details like this double quoting. 

But Cesar Agustin Garcia Vazquez recommends:

>  changing this behavior would break existing code relying on this feature as is. We could add a new option like `select plain_json * from user;` which would have this expected behavior.

There are no more discussion after that.

I think this can be a great improvement for Cassandra. With this "plain_json" feature, people can query information from Cassandra and used it directly in outside projects. 

Nobody touched this issue after 13/Jun/2019. So we would like to give it a try.