# General Instructions

To get started, you must have a GoWatchIt API key. If you do not currently have a GoWatchIt API key, please contact our business team at matt@gowatchit.com.

To get started, you must first include the following script tag right after the open body tag.

```
<script>(function(d, t, id) {
  GWI = { config: {} }
  var js, _s = d.getElementsByTagName(t)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(t);
  js.id = id;
  js.src= 'http://superwidget.gowatchit.com/gwi.js';

  GWI.config.apiKey = 'YOUR_API_KEY';

  _s.parentNode.insertBefore(js, _s);
}(document, 'script', 'gwijslib'));
</script>
```

We will be adding more to this global config as we choose the different types of widgets that we provide.

There are three types, or flavors, of the GoWatchIt widget:

- Availability - show current services where you can watch, own, or rent a given movie or TV show.
- Alert - sign up for an alert for a given movie or TV show to be automatically notified when it will be available on new services.
- Link - display a GoWatchIt-branded button that will link to the movie on GoWatchIt.com (Currently movies only.)

# Configurations

There are some options that you can specify in your global config as well as on the element level.

- categories : Acceptable Values: '3' or '5' (default: '5')
  - Specify the number of categories to display in the availability flavor. If categories is set to '3', then services will be grouped into three categories: 'In Theaters', 'DVD/Blu-ray' and 'Online'. If categories is set to '5', the services will be grouped into five categories: 'In Theaters', 'DVD/Blu-ray', 'Rent/Own Online', 'Online Subscription', 'Free Online'.

- layout: Acceptable Values: 'tabs', 'accordion', 'panels' (default: 'tabs')
  - Specify the layout of the availability flavor. The tabs layout will display each category in a tab, the accordion layout will display each category on top of one another, and only one is visible at a given time, and 'panels' will display all services in each category.

- display: Acceptable Values: 'modal-button', 'inline' (default: 'modal-button')
  - Specify how to display the widget. The 'modal-button' display setting will display a button that when clicked will display a modal. The 'inline' display setting will display the widget within the given html DOM element.

# HTML Embed

Our HTML Embed tags are the simplest way to include the GoWatchIt widget on your webpage. 

## Buttons

### Alert

To include the alert widget on your webpage and to display it as a button that will trigger a modal, just place the following div into your page:

```
<div class='gwi-widget' data-flavor='alert' data-type='movie' data-id='1862' data-display='modal-button'></div>
```

Where the type is either 'movie', 'show', 'season', or 'episode', and the id is the corresponding GoWatchIt ID.

### Availability

**The following will be operational by January 15th, 2015**

To include the availability widget on your webpage and to display it as a button that will trigger a modal, place the following div into your page:

```
<div class='gwi-widget' data-flavor='availability' data-type='movie' data-id='1862' data-display='modal-button'></div>
```

### Simple Link

**The following will be operational by January 15th, 2015**

To render a GoWatchIt-branded button that when clicked will take you to the GoWatchIt movie page, place the following div into your page:

```
<div class='gwi-widget' data-flavor='link' data-type='movie' data-id='1862'></div>
```

Note: The link flavor will always render as a button, and therefore the display attribute does not need to be included.

## Rendering Widgets In Place

**The following will be operational by January 15th, 2015**

### Alert

To display the alert module within a given div, specify the display attribute as 'inline':

```
<div class='gwi-widget' data-flavor='alert' data-type='movie' data-id='1862' data-display='inline'></div>

```

### Availability

To display a given movie or TV show's availability information within a given div, specify the display attribute as 'inline':

```
<div class='gwi-widget' data-flavor='availability' data-type='movie' data-id='1862' data-display='inline'></div>

```

# Javascript SDK

For the greatest flexibility, you can tap into our functionality with the GWI object.

All of our functionality is contained inside the GWI object. Since the javascript include is loaded asynchronously, you must place all of your code using the GWI object in the callback function `goWatchItReady`

```
window.goWatchItReady = function() {
  GWI.display(...)
}

```

*Note:* Some of the sample code uses [jQuery](http://jquery.com), a popular javascript library, to simplify things. If you are testing out these examples on your own, you must include jQuery on your page by placing this within your head tag:

```
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
```

## GWI.showModal

You can programatically call GWI.showModal(options) to render the modal version of the widget. GWI.showModal accepts one parameter, which is a javascript object of options.

Required options:
- type - 'movie', 'show', 'season', or 'episode'
- id - the corresponding GoWatchIt ID.
- flavor - 'availability' or 'alert'

Optional Parameters:
- categories - 3 or 5
- layout - 'tabs', 'accordion', or 'panels'
- animate - A DOM element that will act as the starting point of the Modal animation.

Example Code:

```
window.goWatchItReady = function() {
  $(document).ready(function() {
    $('#my-gwi-widget').on('click', function() {
      var element = $(this).get(0);
      GWI.showModal({
        type: 'movie',
        id: 1862,
        flavor: 'availability',
        animate: element
      })
    })
  })
}
```

That above code snippet will trigger the modal whenever the element with id 'my-gwi-widget' is clicked.

## GWI.display

### Alert

If you wish to display the embed button of the alert button, use the GWI.display(element, options) call. GWI.display takes two parameters, a DOM element to be replaced by the embed button (required), and a javascript object of options. If no options are passed, then the SDK will take the data from the html element, similar to how the HTML embed tag works.

Required options:
- type - 'movie', 'show', 'season', or 'episode'
- id - the corresponding GoWatchIt ID.
- flavor - 'alert'

Optional Options:
- display - 'modal-button' or 'inline' (Currently only modal-button is supported.)

Example Code:

```
JS
---

window.goWatchItReady = function() {
  $(document).ready(function() {
    var element = $('#my-gwi-widget').get(0);
    GWI.display(element, {
      type: 'movie',
      id: 1862,
      flavor: 'alert',
      display: 'modal-button'
    });
  })
}

HTML
---

<div id="my-gwi-widget"></div>
```

If you pass no options to GWI.display, then you must specify the data on the html element. For example:

```
HTML
---

<div id="my-gwi-widget" data-type='movie' data-id='1862' data-flavor='alert'></div>
```

### Availability

**The following will be operational by January 15th, 2015**

You can use GWI.display and pass in the 'availability' as the flavor

Required options:
- type - 'movie', 'show', 'season', or 'episode'
- id - the corresponding GoWatchIt ID.
- flavor - 'availability'

Optional Options:
- display - 'modal-button' or 'inline'
- categories - 3 or 5
- layout - 'tabs', 'accordion', or 'panels'

Example Code:

```
JS
---

window.goWatchItReady = function() {
  $(document).ready(function() {
    var element = $('#my-gwi-widget').get(0);
    GWI.display(element, {
      type: 'movie',
      id: 1862,
      flavor: 'availability',
      display: 'modal-button'
    });
  })
}

HTML
---

<div id="my-gwi-widget"></div>
```


# Simple Button

If you are going to render multiple widgets on a single page, then you should use this simple button implementation, which is much more lightweight than the above examples and will not hamper performance. To see an example of a use case where the simple button should be used, go to [wheretowatch.com](http://wheretowatch.com) and hover over movie and TV show posters to see them in action.

This is done using the GWI.showButton(element, options) function. GWI.showButton takes two parameters, a required DOM element to be replaced by the widget, and a javascript object of options.

Required options:
- type - 'movie', 'show', 'season', or 'episode'
- id - the corresponding GoWatchIt ID.
- flavor - 'availability' or 'alert'

Optional Parameters:
- categories - 3 or 5
- layout - 'tabs', 'accordion', or 'panels'

## Availability

```
JS
---

window.goWatchItReady = function() {
  $(document).ready(function() {
    var element = $('#my-gwi-widget-button').get(0);
    GWI.showButton(element, {
      type: 'movie',
      id: 1862,
      flavor: 'availability'
    });
  })
}


HTML
---

<div id="my-gwi-widget-button"></div>
```

Again, if no options are passed, then the SDK will take the data on the DOM element.

```
JS
---

window.goWatchItReady = function() {
  $(document).ready(function() {
    var element = $('#my-gwi-widget-button').get(0);
    GWI.showButton(element);
  })
}


HTML
---

<div id="my-gwi-widget-button" data-type='movie' data-id='1862' data-flavor='availability'></div>
```


## Alert


```
JS
---

window.goWatchItReady = function() {
  $(document).ready(function() {
    var element = $('#my-gwi-widget-button').get(0);
    GWI.showButton(element, {
      type: 'movie',
      id: 1862,
      flavor: 'alert'
    });
  })
}


HTML
---

<div id="my-gwi-widget-button"></div>
```

## Link Button

To programatically add the GoWatchIt-branded button that will link to a movie's page on GoWatchIt.com, use the following call:

```
JS
---

window.goWatchItReady = function() {
  $(document).ready(function() {
    var element = $('#my-gwi-widget-button').get(0);
    GWI.showButton(element, {
      type: 'movie',
      id: 1862,
      flavor: 'link'
    });
  })
}


HTML
---

<div id="my-gwi-widget-button"></div>
```
Again, this currently only works for movies.
