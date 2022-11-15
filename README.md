# Objective-1

## We will understand here how linux network namespaces communicate each other on deep level

#### Our first goal is create two name space and run python base http server on one namespace and connect from another namespace as client as per below diagram

![imagename](/image/diagram.JPG)

Create 2 name space and show created namespaces

```bash
ip netns add red
ip netns add green
ip netns
```

![imagename](/image/1.JPG)

Create virtual ethernet cable by command

![imagename](/image/2.JPG)

Connect virtual cables to corresponding namespaces

![imagename](/image/3.JPG)

Enter namespace and see interface created

![imagename](/image/4.JPG)

Loopback and virtual ethernet interface up on each namespace

![imagename](/image/5.JPG)

![imagename](/image/6.JPG)

Assign IP addres in each namespace and make sure interface is up

![imagename](/image/7.JPG)

![imagename](/image/8.JPG)

Both namespace interface is up but cannot communicate each other as there is no route on them, from host and name space we can check that namespaces can ping itself but not go outside namespaces.

From host

![imagename](/image/9.JPG)

From namespaces

![imagename](/image/10.JPG)

We need to add route in both namespace

![imagename](/image/11.JPG)

![imagename](/image/12.JPG)

Now we can ping each other namespaces

From namespaces

![imagename](/image/14.JPG)

![imagename](/image/15.JPG)

From hosts

![imagename](/image/13.JPG)

### Install Python service exposed in 9000 port and bind in server (Here it is 192.168.1.1)

![imagename](/image/16.JPG)

Connect from client (Here it is 192.168.1.2)

![imagename](/image/17.JPG)

Sending some text in TCP connection and get responses in server

![imagename](/image/18.JPG)

##### Here we got "code 400, message Bad request syntax" error because TCP connection needs data in http format

# Objective-2

### Our 2nd objective is to connect 3 namespaces in bridge switch and establish communication between them according below diagram

![imagename](/image/bridgeconnection.JPG)


Create bridge switch

```bash
ip link add bri0 type bridge
```
![imagename](/image/19.JPG)


Power up bridge switch

````bash
ip link set bri0 up
````

![imagename](/image/20.JPG)

Set Ip in bridge interface
````bash
ip addr add 192.168.0.1/16 dev bri0
````
![imagename](/image/21.JPG)

Create 3 namespaces
````bash
ip netns add black
ip netns add white
ip netns add green
````
![imagename](/image/23.JPG)

Create 3 virtual cables

```bash
ip link add bveth type veth peer name bbrveth
ip link add wveth type veth peer name wbrveth
ip link add gveth type veth peer name gbrveth
```

![imagename](/image/24.JPG)

Connect virtual cable end to corrosponding namespace

```bash
ip link set bveth netns black
ip link set wveth netns white
ip link set gveth netns green
```

![imagename](/image/25.JPG)

Connect other end of virtual cable to bridge switch port

```bash
ip link set bbrveth master bri0
ip link set wbrveth master bri0
ip link set gbrveth master bri0
```

![imagename](/image/26.JPG)

Power up switch end cables points

```bash
ip link set bbrveth up
ip link set wbrveth up
ip link set gbrveth up
```

![imagename](/image/27.JPG)

Power up namespaces end cables and also up loopback interface

```bash
ip netns exec black ip link set dev bveth up
ip netns exec white ip link set dev wveth up
ip netns exec green ip link set dev gveth up
ip netns exec black ip link set dev lo up
ip netns exec white ip link set dev lo up
ip netns exec green ip link set dev lo up
```

![imagename](/image/28.JPG)

Check Bridge switch and namespaces interface is up now
```bash
ip addr
```

![imagename](/image/29.JPG)

Assign IP address to namespaces interfaces

```bash
ip netns exec black ip addr add 192.168.1.1/24 dev bveth
ip netns exec white ip addr add 192.168.2.1/24 dev wveth
ip netns exec green ip addr add 192.168.3.1/24 dev gveth
`````

![imagename](/image/30.JPG)

Enter on each namespace and add default route command

```bash
ip netns exec green bash
ip route add default dev gveth
```

![imagename](/image/31.JPG)

Now we run python web server on custom port in one namespace
```bash
python3 -m http.server --bind 192.168.1.1 800
```

![imagename](/image/32.JPG)

Now we run tcpdump command on bridge network Python server interface to check connection is transfer over bridge switch

```bash
tcpdump -i bbrveth
```
![imagename](/image/33.JPG)

We also run ping command to Python server from another namespace
```bash
ping 192.168.1.1

```

![imagename](/image/34.JPG)

Now we see traffic passess through bridge switch

![imagename](/image/35.JPG)

![imagename](/image/36.JPG)

Black namespaces marked (B) worked as python server
White namespaces marked (W) worked as Web Client
Green namespaces marked (G) worked as ICMP ping tester
Host Used for monitor traffic

![imagename](/image/37.JPG)

Identify running namespace

````bash
ip netns identify 
````

Identify running namespace

````bash
ip netns identify 
````
![imagename](/image/identify_namespace.JPG)

# Objective-3

#### Our 3rd objective here is to connect namespaces to internet

We can connect other namespaces (192.168.1.1) in same host but ping outside (8.8.8.8)

````bash
ping 192.168.1.1
ping 8.8.8.8
route

````

![imagename](/image/38.JPG)

Set tcpdump on bridge interface and namespace connected to bridge (gbveth) and found that traffic not entering in that interface as their is no route for outside (8.8.8.8) network but can reach other namespaces (192.168.1.1) in same host

![imagename](/image/39.JPG)

![imagename](/image/40.JPG)

Add route for outside (8.8.8.8) network on namespace

````bash
ip route add default via 192.168.0.1
````

![imagename](/image/41.JPG)

After default route add via bridge interface traffic pass switch but no response

![imagename](/image/42.JPG)

Now we need NAT policy in host to send namespaces tarffic to internet
```bash
iptables -t nat -A POSTROUTING -s 192.168.0.0/16 ! -o bri0 -j MASQUERADE
```
![imagename](/image/nat_rule.JPG)

Now we can see 8.8.8.8 response

![imagename](/image/43.JPG)