# Forcing Chrome to "Hard" Reload

Sometimes, when we update a CSS stylesheet, our page appears not to change. This is especially frequent when we're working on static HTML files in the `public/` folder.

The cause is usually Chrome's aggressive "caching", or saving, CSS files that it has already downloaded. If we refresh an HTML page that we've updated, Chrome won't necessarily also refresh all `<link>`ed CSS files unless we ask it to.

To do so, open the Dev Tools, **right-click** on the refresh button, and select "Empty cache and hard reload":

![](/assets/hard-refresh-dev-tools.png)

![](/assets/hard-refresh-right-click-refresh.png)

Your page should now have the latest CSS and any other linked assets (like images or javascripts).
