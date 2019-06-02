# UDP – User Datagram Protocol

## C API

```c
int sock = socket(PF_INET, SOCK_DGRAM, IPPROTO_UDP);

int bytes_sent = sendto(sock, buffer, strlen(buffer), 0,(struct sockaddr*)&sa, sizeof sa);

int recsize = recvfrom(sock, (void *)buffer, sizeof(buffer), 0, (struct sockaddr *)&sa, &fromlen);
```

## Java API

```java
// creates socket and binds it to the given port
DatagramSocket(int port)

// sends the given datagram
send(DatagramPacket p)

// waits for a datagram, the payload will be stored in the given instance
receive(DatagramPacket p)

// creates a datagram
DatagramPacket(byte[] buf, int length, InetAddress address, int port)

// retrieve datagram info
InetAddress getAddress()
int getPort()
byte[] getData()
```

## Messaging patterns

* **Fire-and-forget**: when datagram loss is acceptable.
* **Request-reply**: we send a datagram and wait for an reply. In case multiple requests are sent before replies are received, a token can be sent with each payload to identify the matching reply.
* **Service-discovery**: uses multicast or broadcast.

## Unicast, Broadcast, Multicast

* **Unicast**: packet sent to a single device of a network (defined by an IP address and port).
* **Broadcast**: packet sent to all devices of a network.
* **Multicast**: packet sent to all *interested* devices of a network.

### Broadcast Java Example

```java
socket = new DatagramSocket();
socket.setBroadcast(true);

byte[] payload = "Java is cool, everybody should know!".getBytes();

DatagramPacket datagram = new DatagramPacket(payload, payload.length, InetAddress.getByName("255.255.255.255"), 4411);
socket.send(datagram);
```

### Multicast Java Example

```java
private InetAddress multicastGroup;
MulticastSocket socket = new MulticastSocket(port);
socket.joinGroup(multicastGroup);
```

## Service-Discovery Protocols

* Service provider publish datagram with contact details in multicast group every 5 seconds. A client joins the multicast group and receives the datagrams.
* The service provider joins a multicast group and the laptops send service request datagrams. Printers then contact the laptops individually with contact details.
