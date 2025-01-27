# Chall Description 

Our team of brilliant engineers has developed a highly sophisticated website designed to perform check-ups on other sites. It can even uncover hidden information, possibly concealed by some clever tricksters. Take a look and see if you can find anything!

The problem with the 169.254 subnet was fixed, it was a security issue with the infrastructure.

# Steps

The web site curl an other website but only accept domain names. By the description it seems that it try to hide something on local network.
So we use a `Redirect` to bypass this and access local host :

`https://307.r3dir.me/--to/?url=http://localhost`

At first, we discover nothing but by trying some port we find the flag :

```
https://307.r3dir.me/--to/?url=http://localhost:600

How did you find this? Here is your flag: HACKDAY{Give_ME_YOuR_L0OPb@CK}
```
