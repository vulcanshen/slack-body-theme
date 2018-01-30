# slack-body-theme

Customize your slack body them, For Slack.app (OS X)

## For Slack 3.X

edit **/Applications/Slack.app/Contents/Resources/app.asar.unpacked/src/static/ssb-interop.js**

add following script at very bottom, in my case, after `init(resourcePath, mainModule, !isDevMode);`

```
document.addEventListener('DOMContentLoaded', function() {
  $.ajax({
   url: 'https://raw.githubusercontent.com/shenyun2304/slack-body-theme/master/theme/dark-blue.css',
   success: function(css) {
     $("<style></style>").appendTo('head').html(css);
   }
 });
});
```


## For Slack 2.X

edit **/Applications/Slack.app/Contents/Resources/app.asar.unpacked/src/static/index.js**

1.**setTheme()** function in index.js

```
function setTheme() {
  let webviews = document.querySelectorAll(".TeamView webview");
  const cssURI = 'https://raw.githubusercontent.com/shenyun2304/slack-body-theme/master/theme/dark-blue.css'; // the uri for the css file you want to load
  let cssPromise = fetch(cssURI).then(response => response.text());
  webviews.forEach(webview => {
      webview.addEventListener('ipc-message', message => {
         if (message.channel == 'didFinishLoading')
            // Finally add the CSS in
            cssPromise.then(css => webview.insertCSS(css));
      });
  });
}
```

2.setTheme after **startup()**

```
document.addEventListener("DOMContentLoaded", function() { // eslint-disable-line
  try {
    startup();
    setTheme(); // add this line
   
  } catch (e) {
    console.log(e.stack);

    if (window.Bugsnag) {
      window.Bugsnag.notifyException(e, "Renderer crash");
    }

    throw e;
  }
});
```

3.save index.js

4.quit Slack app and restart

## Using web browser



1.open browser console

> input `allow pasting;` if you're using firefox

2.paste

```
var cssURI = 'https://raw.githubusercontent.com/shenyun2304/slack-body-theme/master/theme/dark-blue.css'; // the uri for the css file you want to load
$.get(cssURI, function(data) {
	let style = document.createElement('style');
    style.innerHTML = data;
    $('head').append(style);
});
```

## Reference

[DrewML/Theming-Slack-OSX.md](https://gist.github.com/DrewML/0acd2e389492e7d9d6be63386d75dd99)
