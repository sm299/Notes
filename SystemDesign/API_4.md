# Application Programming Interface

An API is a set of rules that allows one software application to communicate with another software application.

## Types ->
1. REST API(Representational State Transfer API) -> uses JSON <br>
2. SOAP API(Simple Object Access Protocol API) -> uses XML <br>
3. GraphQL ->Graph Query Language(Provides single endpoint) <br>
4. gRPC -> GRCP Remote Procedural Call(Protocal Buffers)/Google Remote Procedural Call. This is smaller in size than json and xml and faster to transfer <br>
5. WebSockets -> Mostly for notifications, websockets are used.In order to main a chatting stream, whenever we need a real time connection, websockets are used. Unlike the rests, for this whenever front end send request, one channel is established, once this is done, canversation can be done from back end as well.