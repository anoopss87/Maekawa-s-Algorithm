# Maekawa-s-Algorithm
Implement maekawa's mutual exclusion algorithm when multiple client performs write operations concurrently

Design:
Each client will be waiting to accept connection from the initiator. The initiator will send the connect request to all the clients. Once the connection is accepted, the client will set up the socket connections to its quorum set in a separate thread and the main thread will be waiting to accept connections as each client is a request granter. Once this set up is done, each client performs 40 write operations. Before performing write, it executes maekawa’s algorithm to get the permission from all the members of its quorum set. Once the write operation is done, it waits for some time (10-50 ms) and repeat the process. Once 40 write operations are completed, the client sends “done” message to the master server. Once the master receives 7 “done” messages i.e. one “done” message from each of its clients, the master server sends “terminate” message to all the clients and its slave servers and terminate itself. All the clients and the slave servers also terminate on receiving the terminate message.

Run Procedure:
Server:
Compilation: g++ server.cpp –pthread –o server
Server program requires 3 command line parameters. First parameter indicates the port number of the server, second and third parameter indicates the other 2 server’s context.
On dc23 machine (s1):
. /server 8000 s2 s3
On dc24 machine (s2):
. /server 8001 s1 s3
On dc25 machine (s3):
. /server 8002 s1 s2
S1 is mapped to dc23.
S2 is mapped to dc24.
S3 is mapped to dc25

Client:
Compilation: g++ client.cpp –o client
Client program requires 3 common line parameters. First parameter indicate which server to act as a master server, second parameter indicates the port number of master server and third parameter indicates the context of the client.
For e.g.  . /client s1 8000 0,  . /client s1 8000 1,  ./client s1 8000 2, etc.…
Initiator: This has to be the last step i.e. only when 3 servers and 7 clients are started.
Compilation: g++ initiator.cpp –o init
Run:  ./init
Note: 
1)	In the code, s1, s2 and s3 are mapped to dc23 – 8000, dc24 – 8001 and dc25 – 8002 respectively. So the servers has to be run on dc23, dc24 and dc25 machines with the corresponding port numbers. If the server needs to be run on other machines, then the mapping has to be changed in the code (one line change).
2)	In the code, c0 to c6 are mapped to dc30 to dc36. So the clients has to run on those machines.
3)	Initiator can be run on any machine.

