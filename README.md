# Our main objective here is to understand how linux network namespaces communicate each other on deep level

# Our first goal is create two name space and run python base http server on one namespace and connect from another namespace as client as per below diagram

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

# Install Python service exposed in 9000 port and bind in server (Here it is 192.168.1.1)

![imagename](/image/16.JPG)

Connect from client (Here it is 192.168.1.2)

![imagename](/image/17.JPG)

Sending some text in TCP connection and get responses in server

![imagename](/image/18.JPG)

# Here we got "code 400, message Bad request syntax" error because TCP connection needs data in http format

