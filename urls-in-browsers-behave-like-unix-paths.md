# URLs in Web Browsers Behave Like Unix Paths

I think it is typical for web apps to use absolute URLs for all of its links. That is links that start with a slash (/) and specify the full path, beginning at the root, to the page to which you're linking. This is a nice strategy because it will, by default, link to that path on the same domain as whatever page you're viewing. For example, if you're on `superapp.com` and you link to `/about/company/` you'll be taken to `superapp.com/about/company/`. This means your links will preserve https if you're on an https page. It's also nice if you have different subdomains for development like `dev.superapp.com`. There is no need to specify a domain name or scheme in your server side code. It sure gets annoying having to do `{{ app_domain }}/about/company/` for every single link in your app.

Links can also be relative, that is, without the begining slash. In this case the link will be relative to the path you're on. If I'm on `superapp.com/about/` and I link to `company/` that will take me to `superapp.com/about/company/`. This is nice when you've got RESTful URLs and have a page that is a collection of things. What I mean is, let's say you're on a page, `superapp.com/campaigns/5/items/` and you have a list of links to the items. Rather than linking to `/campaign/5/items/1/` and `/campaigns/5/items/2/` you could just link to `1/` and `2/` and not have to duplicate all that path info. If a user hovers over one of these relative links, she'll see the fully computed URL in the browser's status bar; if she hovers over the `2/` link she'll see `http://superapp.com/campaigns/5/items/2/` and not be aware that you are using a relative URL.

If you're thinking to yourself that is kind of like Unix, then you're right: absolute paths start with a slash; relative paths don't. What I haven't seen (but I think is neat) is usage of links that contain `..` or `.` which is even more Unix-ey. Yes these works, but when would you use them?

It's fairly common to use `..` when you're linking to images in CSS files. Usually CSS files and images are served from the same static file server but often in different directories. To traverse these directories without hard-coding the static URL or absolute path, it's conventional to use `..`. If my CSS file is hosted `static.superapp.com/static/css/style.css` and my images are hosted at `static.superapp.com/static/images/` then all the URLs in my CSS will be `../images/sprite.png`. The dot-dot takes me up one level from the `css/` directory and into the `static/` directory. From there I'm taken to the `images/` directory where I find my file.

I've found myself using dot-dot in HTML sometimes, too, particularly in breadcrumbs. Looking back at the campaign example, let's say I'm working on the item page and I want to link back up to the list of items, the campaign instance, and the list of campaigns.
I could do:

    <a href="/campaigns/">Campaigns</a> /
    <a href="/campaigns/{{ campaign_id }}/">{{ campaign_name }}</a> /
    <a href="/campaigns/{{ campaign_id }}/items/">Items</a> /
    {{ item_name }}

or I could do

    <a href="../../../">Campaigns</a> /
    <a href="../../">{{ campaign_name }}</a> /
    <a href="../">Items</a> /
    {{ item_name }}

I think that's a little more readable and it's not necessary to have the campaign_id to build the URL because it's already a part of the URL of the page you're on. Like the other relative link example, the status bar will always show you the absolute URL that this relative URL represents.

You could also use this technique for sibling pages:

    <a href="../317/">Previous</a> --- <a href="../319/">Next</a>

(assuming, of course you're on a page that ends in `318/`.

Now what about the single dot? There are a few places where I could see this useful. One place is for self-submitting forms. By default the action attribute of a form will be the page you're on. Sometimes it's nice to specifically acknowledge that you didn't forget the action attribute by putting one in with a value that's set to empty string. But perhaps it is more explicit if you set the form action to be `action='.'`. That way it seems intentional you want to use the URL of the page you're on. I guess the same could be said for relative links as well. Rather than linking to `2/` you could link to `./2/` to make it more explicit what you're trying to do and that you didn't simply forget the slash.

While the single-dot relative link doesn't offer much of an advantage over the no-slash relative link other than readability, the dot-dot has a lot of advantages. I see them as:

 * Less typing / less bytes.
 * Less variable rendering and URL building.
 * Allows you to add or remove paths higher up the in the tree.
 * Communicates relationships relative to the page you're on instead of relative to the app itself.
 * Useful if you're on an app that has a lot of directories in its paths.

This is something I found myself using recently but haven't really seen it on any other websites so I just wanted to share. Anyone else have thoughts or strategies for managing links in their web apps?
