# Distributed-File-System-Pessimistic


The goal was to design a Distributed File System with replication and disconnection management features. Some operation that our file system included are creating, deleting, restoring, appending and reading a file.

Server Side:

HeartBeat - In order to check if a server was available or not we implemented a feature of sending Heartbeat messages. Every server would send heartbeat messages to remaining servers. If it gets a response then that server is alive. This way we kept track of available servers. 

Working - Whenever client requests for some operation to a server it becomes the primary server. The operation is first performed on primary server and then it sends the operation to replica servers/ remaining servers. There is also a feature called lazy updating we’ve used to sync the servers if they are not. Suppose if a server is down and later it becomes available it may have stale data. In that case it checks with the other servers for the freshness of the data.

Replication - For replication the primary server sends an update request to replica servers. So for client server communication socket connection is used. It’s a one to one connection. But each client connected we created thread that would serve that particular client. But for replication TCP based connection becomes complex. So we decided to go with Remote Method Invocation(RMI). JAVA provides API’s for RMI to access methods in a server.

Concurrent Writes - To prevent concurrent writes issue we use a lease period of 10 mins.  In other words we grant the lease of the file to that client. So other client can’t push the changes to that file. If the client doesn’t make any update to the file in 10 mins, the lease is revoked thereby giving access to other clients.

Logging - All operations are logged using JAVA's Logger class.

Client Side:
While connecting to a server the client check the latency with various servers and connects to the one with lowest latency. The User Interface was the command line

