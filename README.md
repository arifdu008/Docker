

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