# Chall Description 

This website clearly hides a lot of things... be careful, it seems the administrator of this website is monitoring activity! Proceed slowly but surely...

Fuzzing is prohibited on this challenge, same as automatic tools like dirbuster. There's no need of enumeration on this challenge, all have to be done manually. Automatic tools usage can be punished.

# Steps

Looking at the source of an image on the main we can find a `secret` folder :

`/secret_place_for_secrets/steampunk_theme_image.jpg` => `/secret_place_for_secrets/`

Where we find an other page :

```
Index of /secret_place_for_secrets/

../
secreting_the_secret.html                          22-Jan-2025 08:01                2521
steampunk_theme_image.jpg                          22-Jan-2025 08:01              538828
```

In wich there is a form to check for `secret` path, on the source we can discover the source of the form that looks like this :

```javascript
(function(){var _0xa92bf2=["L2cz","YmxfR1g=","ZE1OTQ==","TkROeQ==","WF9V"],_0x9cb483=[0,2,3,1,4];function _0x928acd(_0x1a3c2b){try{var decoded=decodeURIComponent(atob(_0x1a3c2b).split('').map(function(_0x3c48d7){return'%'+_0x3c48d7.charCodeAt(0).toString(16);}).join(''));console.log('Nothing here');return decoded;}catch(e){console.error('Gears, everywhere');return'';}}function _0x18ad3c(){console.log('Yes I like gears');console.log('Everything fine with my gears ?');var _0x47e3bd='';_0x9cb483.forEach(function(_0x2c7d48){var fragment=_0xa92bf2[_0x2c7d48],decoded=_0x928acd(fragment);console.log('Doing some things with JS, but did you find my gear ?');_0x47e3bd+=decoded;});console.log('Oh yes.');return _0x47e3bd;}function _0x32f8d2(){var _0x9f47a3=document.getElementById('_res9384'),_0x3b72d6=document.getElementById('_x284js').value,_0x7c4e19=_0x18ad3c();console.log('User Input:',_0x3b72d6);console.log('Analyzing...');if(_0x3b72d6===_0x7c4e19){_0x9f47a3.innerText='Correct! The path is: '+_0x7c4e19;}else{_0x9f47a3.innerText='Incorrect! Your activity is logged and we will have to find and kill you for trying to find our secrets.';}}document.getElementById('_btn934s').addEventListener('click',_0x32f8d2);})()
```

Looking quickly at it, we have a list in the wrong order and encoded in base64 :

```javascript
_0xa92bf2=["L2cz","YmxfR1g=","ZE1OTQ==","TkROeQ==","WF9V"],_0x9cb483=[0,2,3,1,4]
```

Put it in the good order :

`"L2cz" "ZE1OTQ==" "YmxfR1g=" "ZE1OTQ==" "TkROeQ==" "WF9V"`

And decode each :

`/g3 dMN MNDNy bl_GX X_U`

Going on the path we get our flag :

`challenges.hackday.fr:37784/g3dMNMNDNybl_GXX_U/` => `HACKDAY{ec4975e152bef92b2105681fd80f0c0e}`
