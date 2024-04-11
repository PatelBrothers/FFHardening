# Firefox Hardening Guide
## Arkenfox vs. Narsil
- [GitHub - arkenfox/user.js: Firefox privacy, security and anti-tracking: a comprehensive user.js template for configuration and hardening](https://github.com/arkenfox/user.js)
- [user.js/desktop at main - Narsil/user.js - Codeberg.org](https://codeberg.org/Narsil/user.js/src/branch/main/desktop)

Arkenfox is the most popular `user.js` file that privacy enthusiast use. It is as of writing this, 1/1/2024, well maintained and backed by a large community. However the creator said he does plan to eventually stop maintaining it because of the new Mullvad Browser which out of the box is the most secure browser you can use apart from TOR. Since Narsil, a fork of Arkenfox with a more aggressive emphasis on privacy over security, depends on Arkenfox it will also soon cease to exist. Nevertheless I wanted to try my approach at hardening my own instance of firefox to fit my particular threat model.
### user.js
I ended up deciding on Narsil because I would much rather have more privacy at the cost of less security in this case for two reasons:
- uBlock should be suffice for my security needs
- I am going to be using Startpage for my main search engine which includes their own proxy for opening up webpages incase you are unsure what will be on the other side of a result and want to verify through that

However the install remains the same for both Arkenfox, Narsil, Betterfox, and any other `user.js` files you encounter.
- Open Firefox -> Click the Menu at the top right -> Help -> More Troubleshooting Information -> Profile Folder -> Open Folder -> Paste `user.js` into the directory 

Before I opened back up Firefox I made a few changes to the file to meet my needs:

- I do not want Cookies or Site Data to clear on shutdown so I made that false
```js
// Set "Cookies" and "Site Data" to clear on shutdown
user_pref("privacy.clearOnShutdown.cookies", false); // Cookies
user_pref("privacy.clearOnShutdown.offlineApps", false); // Site Data
```
- I also want my browser to not clear history on shutdown so I made that false
```js
// Enable Firefox to clear items on shutdown
user_pref("privacy.sanitize.sanitizeOnShutdown", false);
```
- Finally I wanted my search bar to default to searching by search engine as soon as I type into it and not defaulting to typing in the exact url so I made that true
```js
// Disable location bar using search
user_pref("keyword.enabled", true);
```

- Save the file and restart Firefox
### Extensions
#### uBlock Origin
- Install uBlock Origin -> Pin the extension -> Select "I am an advanced user" -> Filter lists -> Privacy -> "Block Outsider Intrusion into LAN" -> Update -> Apply
#### Startpage Privacy Protection
- Install the extension -> Set Default Search Engine -> Pin extension -> Set to Night theme 
#### Multi Account Containers
- Install the extension -> added a new harmful container -> Enable bookmark menu in `about:addons` and options for the extension
The harmful container is for sites I am not sure are safe even after I visited it through startpages proxy
# Style

Type `about:config` in the browser and set the following to true if it isn't already:
```css
toolkit.legacyUserProfileCustomizations.stylesheets
```

Open Firefox -> Click the Menu at the top right -> Help -> More Troubleshooting Information -> Profile Folder -> Open Folder -> Make a folder called "chrome" -> in that folder create a file called `userChrome.css` and paste and save the following
```css
/* Removes the blank space at the top left of the browser */

.titlebar-spacer {
	display: none !important;
}

/* Removes the new tab favicon */

.tab-icon-image[src="chrome://branding/content/icon32.png"] {
    display: none !important;
}
```

Rightclick somewhere in the toolbar and click manage toolbar -> remove the flexible space to the left and right of the search bar
# Future Plans
For now I'll continue updating the user.js with my modifications as the updates come but I don't know if this will be my daily driver. Brave is probably the best chromium based solution right now from what I've heard with just needing a few tweaks out of the box so I might look into that for my google accounts. I do want to test if startpages proxy handles things like phishing links now that I have this done as well as maybe add a custom bookmarks page I find on github to replace the boring blank page each time.
