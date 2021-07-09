# Computer Networking

## 1. Latency and Bandwith

### Latency

**What is Latency \(延遲性\)?**  
Latency is 1\) **time until a message arrives**.   
                   2\) the time it takes to communicate from one device to another.

* In the same building/data center: ~ 1ms
* One continent to another: ~ 100ms
* Hard drives in a van: ~ 1 day

### Bandwith

**What is Bandwith \(可乘載的量\)?**  
Bandwith is **data volume per unit time.**

* 3G cellular data: ~ 1 Mb/s
* Home broadband: ~ 10 Mb/s
* Hard drives in a van:  50 TB/box ~ 1Gb/s 

## 2. HTTP and TCP

**How does TCP send data via the Internet?**  
TCP sends network packets \(each packet is a small size.\)   
HTTP uses TCP underneath. \(Request\) TCP breaks down these big messages, into small network packets that are small enough that the network can deliver them. And then on the recipient side \(Response\), TCP puts all of the network packets back again to give us one large chunk of bytes.

## 3. RPC - Remote Procedure Call  

## HTTP Status Code

1. Informational responses \(`100`–`199`\)
2. Successful responses \(`200`–`299`\)
3. Redirects \(`300`–`399`\)
4. Client errors \(`400`–`499`\)
5. Server errors \(`500`–`599`\)

{% embed url="https://developer.mozilla.org/en-US/docs/Web/HTTP/Status" %}



