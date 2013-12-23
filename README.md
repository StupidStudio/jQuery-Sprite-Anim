# jQuery Sprite Anim
#### Version 0.1.8


## Introduction

jQuery Sprite Anim is intended to be a simple sprite animation library. There are already numerous excellent sprite animation libraries available for jQuery. This library differs in a couple of ways, from any of the others we've been able to find:

 * A freely available generator, optimized for this library, has been created.
 * Support for an unlimited number of frames.
 * Creates multiple sprite sheets, to work around size limitations in mobile browsers and some desktop browsers.

 
## Browser support

The library should work well in most modern browsers:

 * Internet Explorer 6+
 * Firefox 1+
 * Google Chrome
 
###### Notes:

 1. Enabling retina currently requires a browser with [background-size](http://caniuse.com/background-img-opts) support.


## License

The MIT License. See LICENSE for further information.


## Generating sprites

### Easy: Use our tool

You may use the webbased generator freely available at <http://apps.stupid-studio.com>. You can simply upload your images and have the tool generate the sprites for you.


### Hard: Do It Yourself

The images must be laid out in sprite sheets, with the first image in the top left corner, the next image right next to it. Once there's no more room in a row, start a new one. There must be no margin between images and all images must have the same size.

The sprite sheets must be saved with a digit, before the extension.


## Getting started

Please see the demo/-folder for a clear example of how to use the library.


## Data attributes

You should simply create a `<div>` with the following data-attributes to get started:

Attribute    | Required | Default value  | Description
:----------- | :------- | :------------- | :-------------------------
data-baseurl | **Yes**  |                | URL to the sprite. Exclude the digit and extension. Example: http://site.com/sprite. Then http://site.com/sprite0.png, http://site.com/sprite1.png etc. will be found automatically.
data-grid    | **Yes**  |                | Example 4x2 means 4 images per row and two rows per sheet.
data-blocksize | **Yes** |               | The size of the images. Example 500x200 means each image in the sprite is 500px width and 200px in height.
data-frames  | **Yes**  |                | How many images does this sprite animation consist of?
data-fps     | No       | 12             | Frames per second.
data-autoplay | No      | true           | Animation will play as soon as initialized and first image is loaded.
data-fowards | No       | true           | Direction of animation - to play backwards, set to false.
data-autoload | No      | true           | As soon as initialized, the first two sheets will be loaded.
data-retina  | No       | false          | Uses background-size and resizes the element, to half size.

Example:

    <div id='mySpriteAnimation'
      data-baseurl='sprite'
      data-grid='4x2'
      data-blocksize='100x50'
      data-frames='123'
      data-fps='12'
      data-autoplay='true'
      data-autoload='true'
      data-retina='false'
    ></div>

Then to initialize, call $.spriteanim() on the DOM element, like so:

    $('#mySpriteAnimation').spriteanim();


## Javascript API

You may call $.spriteanim with one or two arguments. The first is the `action` and the second may contain any additional arguments.

### play

Start playing the animation. Takes no arguments.

### stop

Stop playing the animation. Takes no arguments.

### fps

Changes the framerate. You must supply a second argument with the intended FPS.

If the animation is playing:
 * The animation will continue playing at the new FPS.

If the animation is not playing:
 * The animation will remain stopped.

Example:

    $('#mySpriteAnimation').spriteanim('fps', 24); // change the framerate to 24

### forwards

Changes the play direction of the animation. You must supply a second argument, which is a boolean.

If the argument is true, the animation will play forwards. If false, it will play backwards.

If the animation is not playing, it will remain not playing.

### frame

Changes the animation frame and stops playback. You must supply a second argument with the intended frame. 

Example:
    
    $('#mySpriteAnimation').spriteanim('frame', 16); 

## Javascript Events

Various events are dispatched and can be used to finetune the animation as you please.

Implementation is through the standard jQuery Events on the element.

Some events can be cancelled by calling preventDefault() on the event-object, which is always given as the first argument.

Example:

    // stop the animation the next time it is supposed to start playing
    $('#mySpriteAniamtion').one('play', function(ev) { ev.preventDefault(); })

### play

Called everytime the animation is set to start playing. This is not called when FPS is changed.

**Cancelable?** Yes.


### frame-`X`-show

Replace `X` with a frame number, 0-indexed. Called just before changing to the frame.

`X` may also be one of the following special keywords:

 * last: Called on the last frame of the animation.

**Cancelable?** Yes. Note: This also stops the animation.


### frame-`X`-shown

Replace `X` with a frame number, 0-indexed. Called just *after* changing to the frame.

`X` may also be one of the same special keywords from above.

**Cancelable?** Somewhat: The animation will stop playing, but the frame has changed and so does not revert to previous frame.


### sheet-loaded

A sprite sheet has been loaded. Please note: This may be triggered a lot, as loading occurs each time we change sprite sheet.

**Cancelable?** No.


### sheet-`X`-loaded

Replace `X` with a sheet number, 0-indexed. A specific sprite sheet has been loaded.

Please note: This may be triggered a lot, as loading occurs each time we change sprite sheet.

**Cancelable?** No.


## Change log

###### v0.1.8

*Thanks to [NeilCross](https://github.com/NeilCross) with a perfect  [pull request](https://github.com/StupidStudio/jQuery-Sprite-Anim/pull/5), which solely makes up this revision.*

A feature add and a few bug fixes:

 * Add support for display of a specific frame, with the format .spriteanim('frame',n) to display frame n
 * Fixed multiple calls to 'play' creating more than one executing loop, and resolved issue that FPS was not applied immediately; both as the timer object was never set.
 * Make animations stop immediately, rather on next frame iteration - resolves issue with needing to call play twice twice after calling 'stop' twice.

###### v0.1.7

 * Bugfix: Better detection of background-image-change.
 * Bugfix: when only one sprite-sheet is needed.

###### v0.1.6a

 * Bugfix: prepareNextSheet reverted to 0.1.4a-version, as the last sheet preparation broke.

###### v0.1.5a

 * Now supports playing backwards.

###### v0.1.4a

 * Now uses requestAnimationFrame where available.

###### v0.1.3a

 * Fixed: Incorrect background-size calculation on small sheets.

###### v0.1.2a

 * If the element has been removed from the DOM, the animation stops itself.

###### v0.1.1a

 * Included polyfills, tested and now works in Internet Explorer 6+.

###### v0.1a

 * Initial release


## Feature wishlist

 * Currently the library only supports PNG sprites. Should support most common web image formats.
 * Support for reversing playback.
 * Support for programmatically skipping to a specific frame.
 * Support other high-resolution resolutions, instead of just "retina" (2x).
 * Background-size polyfill for older browsers.
