# You're so vain
## You just want more pictures of you

So, you're tired of Photo Booth and you want something more "cross-platform".
Specifically, you __really__ want to be able to take pictures of yourself in your browser,
because, well, who doesn't, right?

Well, now you can!

HTML5 introduced a [slew of new APIs](http://www.sitepoint.com/10-html5-apis-worth-looking/)
that do a variety of things. But the one we're interested in today is
[getUserMedia](https://developer.mozilla.org/en-US/docs/NavigatorUserMedia.getUserMedia).
This newfangled API affords meer web developers, like you and I, the ability to
capture audio and video through __native browser code__. That's right, no more
"You must install Flash" messages or "It's time for another update of Silverlight!".
Just a sprinkling of Javscript, and away we go.

Now, let's assume we have a barebones HTML page:

```html5
<!DOCTYPE html>
<html>
<head>
<!-- BLAH BLAH BLAH -->
</head>
<body>
  <video id='vid'></video>
  <canvas id='pic' width='640' height='480'></canvas>
  <button type='button' id='take-pic'>Smile!</button>
</body>
</html>
```

And we want to take a picture of ourselves. How do we go about it? Throw the following
in a `script` tag at the end of the body and you'll be bright and vain ... er,
right as rain.

```javascript
var vid, pic, button, media, takePicture;
vid = document.getElementById('vid');
pic = document.getElementById('pic');
button = document.getElementById('take-pic');

takePicture = function takePicture() {
  pic.getContext('2d').drawImage(vid, 0, 0, 640, 480);
  var data = pic.toDataURL('image/png');
  pic.setAttribute('src', data);
};

button.onclick = takePicture;

navigator.webkitGetUserMedia(
  {video: true, audio: false},
  function success(stream) {
    media = stream;
    vid.src = window.URL.createObjectURL(media);
    vid.play();
  },
  function error(err) {
    alert('Oh noes!');
  }
);
```

The full page is on [Github](https://github.com/dugancathal/bright-and-vain), feel free to clone and fiddle all you want.

P.S. Keep in mind that this will only work in Chrome, you'll have to do some 
vendor shimming if you want it to work in other browsers. Google keeps an
[adapter](https://github.com/GoogleChrome/webrtc/blob/master/samples/web/js/adapter.js)
available for just this purpose.
