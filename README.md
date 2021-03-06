# LaBonneCarte
_A [**chrome extension**](https://chrome.google.com/webstore/detail/la-bonne-carte/oegacpncaonolgbpmphcimodilfoacnl?hl=fr) to display leboncoin's (a french craigslist) search results on a Google map._

Leboncoin is the main website to buy and sell stuff in France, but for some reason, there is no way to display all the results of your search directly on a map. It was annoying so, in a free afternoon, I decided to fix it by creating my first chrome extension.

Basically, the extension parses a Leboncoin html page to gather the page's product list data (title, price... and more importantly the location), and, if pertinent, displays a google maps on top of the page.

![Screenshot of LaBonneCarte in action](http://tof.canardpc.com/view/514e0085-4680-4579-a63d-cbeaa2b7417e.jpg)

## How is the code working?
_Real brief explanation, if you want more information feel free to contact me._

The main part of the extension is the contentmodifier.js, a _content_scripts_ running on all pages of leboncoin.fr. 
On every page load, it will try to gather the product list from the page. If it succeeds, it means that we are in a search list page.
- We then use chrome messaging to store the current item list on the background.js file : the persistent file of the extension.
- And we inject an iframe on top of the page, loading our mapviewer.html.

Our mapviewer.html will then
- load the Google maps API
- create and display the map
- get the item list from the background.js, create a map marker and infowindow for every item.

A cache for the geocode requests as been implemented on the background file, as the Google Geocode API has a [strict quota policy](https://developers.google.com/maps/documentation/geocoding/usage-limits).

Please note the _permissions_ and _content_security_policy_ in the manifest.json file. Calls to external APIs (even Google ones) won't work without thoses lines. 
_web_accessible_resources_ is also necessary to inject images or code or html into the target page.

### What's left to do?
*Please feel free to participate or improve the code.*
- [x] Correct the map markers overlap when two items are in the same city.
- [x] Central Paris area are not really working (* as the page displays things like Paris 15ème...*). _Edit: it now works. Magic!_
- [ ] Better architecture.
- [ ] Unit tests.

### Disclaimer
- I am not affiliated at all with leboncoin.
- The code was just a fun project for me, in order to learn how to make Chrome extensions. It is not really in a clean production state (no tests, no real architecture...). Use it at your own risk.
- Some sentences of the extension are in french only as the target website is french only.
