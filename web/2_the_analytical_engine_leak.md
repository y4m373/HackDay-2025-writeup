# Chall Description 

In the shadow of the huge copper chimneys, a dark plot is brewing. The engineers of the Inventors' Guild have developed a revolutionary device: a steam-powered analytical machine capable of deciphering all secret codes. But before it could be activated, a group of cyber-saboteurs, the Black Mist, infiltrated its network to steal the plans. Fortunately, the plans were encrypted.

An allied spy intercepted a trail leading to the Steam Station's digital archives, where a secret database stores crucial information for deciphering the device's plans. However, access is restricted, and only a few people can extract the contents.

Your mission: exploit a flaw in the system to recover the encryption key before it falls into the wrong hands. To avoid alerting an archivist, the use of automatic tools is prohibited. Manual exploitation only.

# Steps

For this one, I just saw a form and when I put `' OR 1=1;` in the login part it gave me the list of user :

```
1 admin
2 engineer
3 researcher
4 cipher
5 guest
6 engineer2
7 archivist
```

So I just used sqlmap and got the flag (learn after that this wasn't allowed)

For this replay a `POST` request on the form in Burp Suite, then save it as `req.txt`.

Then use sqlmap : `sqlmap -r req.txt -dbs -dump`
