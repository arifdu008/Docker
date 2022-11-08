# Our main objective here is to understand how linux network namespaces communicate each other on deep level

### _Our first goal is create two name space and run python base http server on one namespace and connect from another namespace as client as per below diagram_

![imagename](/image/diagram.JPG)

Create 2 name space and show created namespaces

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

## Install Python service exposed in 9000 port and bind in server (Here it is 192.168.1.1)

![imagename](/image/16.JPG)

Connect from client (Here it is 192.168.1.2)

![imagename](/image/17.JPG)

Sending some text in TCP connection and get responses in server

![imagename](/image/18.JPG)

### Here we got "code 400, message Bad request syntax" error because TCP connection needs data in http format

# We will now connect 3 namespaces in bridge switch and communicate between them according below diagram




Create bridge switch

![imagename](/image/19.JPG)

Power up bridge switch

![imagename](/image/20.JPG)

Set Ip in bridge interface

![imagename](/image/21.JPG)

Create 3 namespaces

![imagename](/image/23.JPG)

Create 3 virtual cables

![imagename](/image/24.JPG)

Connect virtual cable end to corrosponding namespace

![imagename](/image/25.JPG)

Connect other end of virtual cable to bridge switch port

![imagename](/image/26.JPG)

Power up cables switch end

![imagename](/image/27.JPG)

Power up cables namespaces end and also up loopback interface

![imagename](/image/28.JPG)

Bridge switch and namespaces interface is up now

![imagename](/image/29.JPG)

Assign IP address to namespaces interfaces

![imagename](/image/30.JPG)

Enter in each namespace and add default route command

![imagename](/image/31.JPG)

Now we run python web server on custom port in one namespace

![imagename](/image/32.JPG)

Now we run tcpdump command on bridge network Python server interface to check connection is transfer over bridge switch

![imagename](/image/33.JPG)

We also run ping command to Python server from another namespace

![imagename](/image/34.JPG)

Now we see traffic passess through bridge switch

![imagename](/image/35.JPG)

![imagename](/image/36.JPG)

Black namespaces marked (B) worked as python server
White namespaces marked (W) worked as Web Client
Green namespaces marked (G) worked as ICMP ping tester
Host Used for monitor traffic

![imagename](/image/37.JPG)

