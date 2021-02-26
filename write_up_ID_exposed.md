## ID exposed Write up

#### Challenge description
```
We are looking for Sara Medson Cruz's last location, where she left a message.
We need to find out what this message is! We only have her email: saramedsoncruz@gmail.com
```

For this challenge i started with a simple google search and couldnt find anything in particular. 

Then i decided to use Sherlock, where i found something:

```python
╰─$ python3 sherlock saramedsoncruz                                            
[*] Checking username saramedsoncruz on:
[+] Chess: https://www.chess.com/member/saramedsoncruz
[+] NameMC (Minecraft.net skins): https://namemc.com/profile/saramedsoncruz
[+] Pinterest: https://www.pinterest.com/saramedsoncruz/
[+] ProductHunt: https://www.producthunt.com/@saramedsoncruz
[+] Spotify: https://open.spotify.com/user/saramedsoncruz
[+] Steamid: https://steamid.uk/profile/saramedsoncruz
[+] Tinder: https://www.gotinder.com/@saramedsoncruz
[+] Twitter: https://mobile.twitter.com/saramedsoncruz
[+] geocaching: https://www.geocaching.com/p/default.aspx?u=saramedsoncruz
```

I ttried to follow the links, but didint end up finding anything.

I then searched a while on google and found this article: https://medium.com/week-in-osint/getting-a-grasp-on-googleids-77a8ab707e43

Its about how to retrieve information from the google people api.

if you add a person as a contact in https://contacts.google.com and then inspects the `data-personid` we can use that id at: https://developers.google.com/people/api/rest/v1/people/get to test the google api for information

in the resourceName we enter `people/c1969533330561162561` and in the persnFields we want to get metadata

the api response is: 

```JSON
{
  "resourceName": "people/c1969533330561162561",
  "etag": "%EgMBNy4aBAECBQciDFl0TWlpT1NzcEVJPQ==",
  "metadata": {
    "sources": [
      {
        "type": "CONTACT",
        "id": "1b5530210d63dd41",
        "etag": "#YtMiiOSspEI=",
        "updateTime": "2021-02-26T09:41:12.161Z"
      },
      {
        "type": "PROFILE",s
        "id": "117395327982835488254",
        "etag": "#4eZz2/IuMFw=",
        "profileMetadata": {
          "objectType": "PERSON",
          "userTypes": [
            "GOOGLE_USER"
          ]
        }
      }
    ],
    "objectType": "PERSON"
  }
}
``` 
where we can use this line:
`"id": "117395327982835488254"`

and since we want to find her location, we could use gmail:
https://www.google.com/maps/contrib/117395327982835488254
And we can see where she uploaded a picture last time.

By reading the reveiews from her, we can see the flag:


