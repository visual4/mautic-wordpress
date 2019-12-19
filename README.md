Mautic WordPress plugin (fork)
=======================

[Mautic](http://mautic.org) [Wordpress Plugin](https://wordpress.org/plugins/wp-mautic/) injects Mautic tracking script and image in the WP website. Your Mautic instance will be able to track information about your visitors. You can also insert Mautic content inside your website using different shortcodes.

## Key features
- You don't have to edit source code of your template to insert tracking code.
- Plugin adds additional information to tracking image URL so you get better results than using just plain HTML code of tracking image.
- You can use Mautic form embed with shortcode described below.
- You can choose where the script is injected (header / footer).
- Tracking image can be used as fallback when JavaScript is disabled.

## Installation

### Manually, as this is a fork of WP Mautic
the two default methods described below will not work for this fork of the plugin. To get this fork installed, you'll have to create a zip file and do a manual upload to your WP instance
create a zip file with the following files/folders:

wp-mautic
 - index.php
 - options.php
 - shortcodes.php
 - wpmautic.php

### Via WP administration

Mautic - WordPress plugin [is listed](https://wordpress.org/plugins/wp-mautic/) in the in the official WordPress plugin repository. That makes it very easy to install it directly form WP administration.

1. Go to *Plugins* / *Add New*.
2. Search for **WP Mautic** in the search box.
3. The "WP Mautic" plugin should appear. Click on Install.

### Via ZIP package

If the installation via official WP plugin repository doesn't work for you, follow these steps:

1. [Download ZIP package](https://github.com/mautic/mautic-wordpress/archive/master.zip).
2. At your WP administration go to *Plugins* / *Add New* / *Upload plugin*.
3. Select the ZIP package you've downloaded in step 1.

## Configuration

Once installed, the plugin must appared in your plugin list :

1. Enable it.
2. Go to the settings page and set your Mautic instance URL.
3. you will find two extra options for script integration under header/footer:
- "Only script embedded within footer" - this only loads mtc.js and adds an object mt_attrs. The tracking itself can be called by google tag manager and may contain the extra attributes in the form of 
```html
mt('send', 'pageview', mt_attrs);
```
- "Do not embed Mautic script" this will embed no mautic scripts at all, tracking can be handled by another plugin or Google Tag Manager. 
All short codes should work like normal - for some it might be necessary that mtc.js is embedded elsewhere.

And that's it !

## Usage

### Mautic Tracking Script

Tracking script works right after you finish the configuration steps. That means it will insert the `mtc.js` script from your Mautic instance. You can check HTML source code (CTRL + U) of your WP website to make sure the plugin works. You should be able to find something like this:

```html
<script>
    (function(w,d,t,u,n,a,m){w['MauticTrackingObject']=n;
        w[n]=w[n]||function(){(w[n].q=w[n].q||[]).push(arguments)},a=d.createElement(t),
        m=d.getElementsByTagName(t)[0];a.async=1;a.src=u;m.parentNode.insertBefore(a,m)
    })(window,document,'script','http://yourmauticsite.com/mtc.js','mt');

    mt('send', 'pageview');
</script>
```

#### Custom attributes handling

If you need to send custom attributes within Mautic events, you can use the `wpmautic_tracking_attributes` filter.

```php
add_filter('wpmautic_tracking_attributes', function($attrs) {
    $attrs['preferred_locale'] = $customVar;
    return $attrs;
});
```

The returned attributes will be added to Mautic payload.

### Mautic Forms

To load a Mautic Form to your WP post, insert this shortcode to the place you want the form to appear:

```
[mautic type="form" id="1"]
```

Replace "1" with the form ID you want to load. To get the ID of the form, go to your Mautic, open the form detail and look at the URL. The ID is right there. For example in this URL: http://yourmautic.com/s/forms/view/3 the ID is 3.

### Mautic Focus

To load a Mautic Focus to your post, insert this shortcode to the place you want the form to appear:

```
[mautic type="focus" id="1"]
```

Replace "1" with the focus ID you want to load. To get the ID of the focus, go to your Mautic, open the focus detail and look at the URL. The ID is right there. For example in this URL: http://yourmautic.com/s/focus/3.js the ID is 3.

### Mautic Dynamic Content

To load dynamic content into your WP content, insert this shortcode where you'd like it to appear:

```
[mautic type="content" slot="slot_name"]Default content to display in case of error or unknown contact.[/mautic]
```

Replace the "slot_name" with the slot name you'd like to load. This corresponds to the slot name you defined when building your campaign and adding the "Request Dynamic Content" contact decision.

### Mautic Gated Videos

Mautic supports gated videos with Youtube, Vimeo, and MP4 as sources.

To load gated videos into your WP content, insert this shortcode where you'd like it to appear:

```
[mautic type="video" gate-time="#" form-id="#" src="URL"]
[mautic type="video" src="URL"]
```

Replace the # signs with the appropriate number. For gate-time, enter the time (in seconds) where you want to pause the video and show the mautic form. For form-id, enter the id of the mautic form that you'd like to display as the gate. Replace URL with the browser URL to view the video. In the case of Youtube or Vimeo, you can simply use the URL as it appears in your address bar when viewing the video normally on the providing website. For MP4 videos, enter the full http URL to the MP4 file on the server.

Since the Mautic v2.9.1 release, the form-id is not mandatory anymore, mautic video can be tracked.

### Mautic Tags

You can add or remove multiple lead tags on specific pages using commas. To remove an tag you have to use minus "-" signal before tag name:

```
[mautic type="tags" values="mytag,anothertag,-removetag"]
```
