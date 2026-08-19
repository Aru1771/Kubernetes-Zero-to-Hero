What is the DNS ?
=================

DNS: is the Domine Name System.

* It will act like a PhoneBook to our Internet.
* If i serch for a google.com in the browser then this browser will check the IP address of the Google.con in the DNS.
* If the Ip was foung it will send it back to browser. so the browser will know this is the Ip addrees of the domine. i have to forword this
  request to thet server.

* these above statements are simple but in real world it not that much easy. why because we have lot of websites and we have lot of users over the
  world.
* one dns server will not bare the enire load.
* and another issue every time when we are requesting the website everytime it will always go to DNS and do all the things mentioned above.
* due that we experions a latency.

* To over come this issue we have catche. DNS hava catche, our browser have a catche, our os have a catche.
* Means only the first time when we are accessing the website will do check with DNS server and for rest all the 2, 3, etc access request will
  use these catches.

* one of the issue was resolved with the help of catche. But how we can trust a single server for billions of request.
* we can't trust a single server.
* in real world we have 13 root ns servers to accepts these internet requests.
* these 13 will acts like a load balancer internally they have some many servers.

* if you make a request for a particualr wibsite then the request will to near by root NS.

* Take an eg:
  www.Aravind.dev

  www --> Subdomine
  .dev --> is top level domine

  * we have a separate servers for Top Level domines.
  * For .com we have one server , for .dev we have one server, for .org we have one server.
  * In this case my top level domine is .dev so the Root NS will route the request to .dev server.
  * Then the request will go From Top level server to a Autherity name server like go dady, route53, google domine.
  * and there it will request will resolve.



  * In DNS we have some types of records:
    -------------------------------------
    1. A record  ---> IPV4 Address
    2. AAAA record ---> IPV6 Address
    3. C Name Records
    4. NS Records -- learn in feature
    5. HTTPS Records
       and a lot more

   1. A record: if you add A record in DNS then this A records will forword our request to a backend server based up on the Domine name.
 
      Type : A
      Name: @ --> root domine with out any subdomine
      IPV4 Address: our backedn server IP.

 2. C Name: By using this C Name records we will point our SUB-domines to our root Domine.
    Eg:
    my domine is aravind.dev
    Type: CNAME
    in the C name records will add a subdomine Magic.aravind.dev
    then this subdomine point out to our root domine aravind.dev A Record.
    
      
      
