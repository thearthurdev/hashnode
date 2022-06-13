## Sharing info-rich links of your Flutter Website using Meta Tags

## It's just the link?
So let's say you've finished building that beautiful portfolio website with Flutter Web. You've hosted it on your favorite hosting service and now you want to invite people to check it out. You see a great opportunity on your Twitter timeline and reply with a link to the website. 

This is what it looks like...

![Screenshot of tweet with a link to thearthur.dev](https://cdn.hashnode.com/res/hashnode/image/upload/v1618687808272/QvTQKpfx5.jpeg)

Yes, *it's just the link*. Your website's link is shared just as it is. There's no preview image, nor is there a title or description. It isn't eye-catching enough and you don't get as many interactions and feedback as you'd hoped.

Thankfully there's a way to fix this, and you won't even need to resend your link anywhere you might have shared it.
  
## Open Graph meta tags
The [Open Graph protocol](https://ogp.me/) gives a website the ability to become a rich object in a social graph. What this simply means is that social websites or apps can take advantage of Open Graph meta(data) tags to provide rich information about a website when its link is shared to those sites or apps. These sites may sometimes make the link behave similarly to other objects on the site.

There are a lot of OG meta tags that can be used to enrich your website and all of them can be found [here](https://ogp.me/). However, to achieve the goal of this post we're only going to need a select few.

- `<meta property="og:title" content="ArthurDev">`
- `<meta property="og:description" content="My Portfolio Website">`
- `<meta property="og:url" content="https://thearthur.dev/">`
- `<meta property="og:image" content="https://raw.githubusercontent.com/thearthurdev/arthurdev/master/web/images/social_share_image.png">`


## Twitter card tags
The basic OG meta tags will work for most websites and apps, but Twitter has its meta tags that it uses to create rich and interaction-worthy cards for your links.

Once again we only need a few of the Twitter card tags for the scope of this post.

- `<meta name="twitter:card" content="summary_large_image">`
- `<meta name="twitter:title" content="ArthurDev">`
- `<meta name="twitter:description" content="My Portfolio Website">`
- `<meta name="twitter:site" content="@thearthurdev">`
- `<meta name="twitter:image" content="https://raw.githubusercontent.com/thearthurdev/arthurdev/master/web/images/social_share_image.png">`

Find out all you need to know about these and the remaining tags and what they do in the [docs](https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/abouts-cards). 

They can be used in combination with OG meta tags and there's even a helpful [table](https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/markup) in their docs which shows the Open Graph alternatives to some of their tags so you don't have redundant tags in your `index.html` file.


## How to use dem tags
Speaking of the `index.html` file, that's where you place your meta tags. When you build your Flutter website for the first time it generates an `index.html` file in your `[project_name]/web` folder, and never touches that file again. 

Your meta tags are placed `<head> inside your head tags </head>` and that's about it.

```
<!DOCTYPE html>
<html>

<head>
  <meta charset="UTF-8">
  <meta content="IE=Edge" http-equiv="X-UA-Compatible">
  <meta name="description" content="Personal website and portfolio for ArthurDev.">

  <!-- iOS meta tags & icons -->
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black">
  <meta name="apple-mobile-web-app-title" content="arthurdev">
  <link rel="apple-touch-icon" href="icons/Icon-192.png">

  <!-- Meta Tags for twitter sharing -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:site" content="@thearthurdev">

  <!-- Meta Tags for other social media sharing -->
  <meta property="og:title" content="ArthurDev">
  <meta property="og:description" content="Meet Delords Arthur. Software Developer, based in Ghana.">
  <meta property="og:image" content="https://raw.githubusercontent.com/thearthurdev/arthurdev/master/web/images/social_share_image.png">
  <meta property="og:url" content="https://thearthur.dev/">

  <!-- Favicon -->
  <link rel="shortcut icon" href="favicon.ico" type="image/x-icon">
  <link rel="icon" href="favicon.ico" type="image/x-icon">

  <title>ArthurDev</title>
  <link rel="manifest" href="manifest.json">
</head>

<body>
  <!-- This script installs service_worker.js to provide PWA functionality to
       application. For more information, see:
       https://developers.google.com/web/fundamentals/primers/service-workers -->
  <script>
    if ('serviceWorker' in navigator) {
      window.addEventListener('load', function () {
        navigator.serviceWorker.register('flutter_service_worker.js');
      });
    }
  </script>
  <script src="main.dart.js" type="application/javascript"></script>
</body>

</html>
```

## Force re-caching
So you're done adding your meta tags. You've rebuilt and re-deployed your website and you go to share the link again. *It's still just the link. What?!*

Don't panic. Your tags will work, it's just a matter of time for the social website's or app's crawlers to find your website, extract its metadata, and cache it.

However, if you want to expedite this process there are a few ways to do it. So far I've found the solutions for [Twitter](https://webmasters.stackexchange.com/a/126166) and [Telegram](https://stackoverflow.com/a/35793374/8274769). There are bound to be other ways to force re-caching for other social websites or apps.

## The result
If everything works at it should, all links to your Flutter website that you previously shared will magically be transformed into beautiful and info-rich cards that are sure to boost its interaction metrics.


![Screenshot of tweet with info card linking to thearthur.dev](https://cdn.hashnode.com/res/hashnode/image/upload/v1618746918203/_ySJHQufh.jpeg)

**Enjoy your day**

\- ArthurDev
