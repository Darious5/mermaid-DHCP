```mermaid
sequenceDiagram
    participant server1
    participant client
    participant server2
    note over server1:UP
    note over server2:UP
    note over client:The client discovers the servers in the network by broadcast
    client->>server2: dhcpdiscover
    client->>server1: dhcpdiscover
    note over client:The servers send the ip,mask and concesion time of each one
    server2->>client:dhcpoffer
    server1->>client:dhcpoffer
    note over client:The client broadcasts what ip is going to chose the one who doesnt correspond denies
    client->>server2: dhcprequest (denied)
    client->>server1: dhcprequest
    note over client:The server that got the same ip as the clients broadcast acknowledges his configuration
    server1->>client:dhcpack
    note over client:there is an 3 minute outage, the client reconnects but server 1 is down

    note over server1:DOWN
    note over client:the clients sends a request to server 1, because the lease time did not expire
    client->>server1:dhcprequest
    note over server1:cannot respond
    note over client:the client realizes that it did not connect so it does a full DHCP protocol
    note over client:The client discovers the servers in the network by broadcast
    client->>server2: dhcpdiscover
    note over client:The servers send the ip,mask and concesion time of each one
    server2->>client:dhcpoffer
    note over client:The client broadcasts what ip is going to chose, now there is only one
    client->>server2: dhcprequest 
    note over client:The server that got the same ip as the clients broadcast acknowledges his configuration
    server2->>client:dhcpack

    note over client:50% of the lease time passes
    note over client: the client asks if he can still use the ip
    client->>server2:dhcprequest
    note over client:in this case he can so server 2 sends a DHCPACK
    server2->>client:dhcpack

    note over client:the lease time runs out so the client needs to restart the DHCP protocol from the begining
    note over client:The client discovers the servers in the network by broadcast
    client->>server2: dhcpdiscover
    note over client:The servers send the ip,mask and concesion time of each one
    server2->>client:dhcpoffer
    note over client:The client broadcasts what ip is going to chose, now there is only one
    client->>server2: dhcprequest 
    note over client:The server that got the same ip as the clients broadcast acknowledges his configuration
    server2->>client:dhcpack

    note over client:50% of the lease time passes
    note over client: the client asks if he can still use the ip
    client->>server2:dhcprequest
    note over client:in this case he can so server 2 sends a DHCPACK
    server2->>client:dhcpack
    note over client:the client shutsdown the computer for 1 hour and comes back
    note over client:the client sends a dhcp request to the server2 because the lease time did not expire
    client->>server2:dhcprequest
    note over client:the server2 responds with a DHCPACK
    server2->>client:dhcpack
    note over client:the user spends 3 hours and gets to 50% of the lease time, so he has to send a dhcprequest to server2
    client->>server2:dhcprequest
    note over client:the server2 responds with a DHCPACK
    server2->>client:dhcpack
    note over client:the client spends 2 more hours, because it does not exceed the lease time, nothing happens
    note over client:the client disconnects permanently
    ```