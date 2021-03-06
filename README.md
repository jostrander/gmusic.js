# gmusic.js [![Build Status](https://travis-ci.org/gmusic-utils/gmusic.js.svg?branch=master)](https://travis-ci.org/gmusic-utils/gmusic.js)
Browser-side JS library for controlling [Google Music][].

[Google Music]: https://play.google.com/music/

This was built as part of [google-music-webkit][], a [node-webkit][] wrapper around [Google Music][]. It was forked from [radiant-player-mac@v1.3.1][], developed and created by [Sajid Anwar][] and [James Fator][] to make it reusable and well tested.

`gmusic.js` is not created by, affiliated with, or supported by Google Inc.

[google-music-webkit]: https://github.com/twolfson/google-music-webkit
[node-webkit]: https://github.com/rogerwang/node-webkit
[radiant-player-mac@v1.3.1]: https://github.com/kbhomes/radiant-player-mac/tree/v1.3.1
[Sajid Anwar]: https://github.com/kbhomes/
[James Fator]: http://jamesfator.com/

## Breaking changes in 5.0.0
The method `toggleRepeat()` no longer accepts arguments and `setRepeat(mode)` has replaced its functionality.

## Breaking changes in 3.0.0
On Thursday May 14, 2015 Google launched a Material redesign of the site. This broke a lot of selectors/functionality. In 3.0.0, we updated our integration to handle those changes. The developer-facing interface has not changed but the underlying system was a breaking change so we decided to make it a major release.

## Getting Started
### npm
Install the module with: `npm install gmusic.js`

```js
// Load and initialize GMusic
var GMusic = require('gmusic.js');
window.gmusic = new GMusic(window);

// Access volume
window.gmusic.volume.getVolume(); // 50 (ranges from 0 to 100)
```

### bower
Install the module with: `bower install gmusic.js`

Once installed, add it to your HTML and access it via `window.GMusic`.

```html
<script src="bower_components/gmusic.js/dist/gmusic.min.js"></script>
<script>
  window.gmusic = new window.GMusic(window); // Our Google Music API
</script>
```

### Vanilla
If you are not using a package manager, download the latest script at:

https://raw.githubusercontent.com/gmusic-utils/gmusic.js/master/dist/gmusic.min.js

Then, add it to your HTML and access it via `window.GMusic`.

```html
<script src="gmusic.min.js"></script>
<script>
  window.gmusic = new window.GMusic(window); // Our Google Music API
</script>
```

## Documentation
`gmusic.js` exposes a constructor, `GMusic` as its `module.exports` (`window.GMusic` for `bower`/vanilla).

### `new GMusic(window)`
Constructor for a new Google Music API. For usage with `node-webkit`, we require `window` to be passed in rather than assumed from global scope.

- window `Object` - Global `window` object for target browser window

### Volume
`gmusic.volume` exposes interfaces to the volume controls of Google Music. Volume can range from 0 to 100 in steps of 5 (e.g. 10, 15, 20).

#### `volume.getVolume()`
Retrieve the current volume setting

**Returns:**

- retVal `Number` - Integer from 0 to 100 representing volume

#### `volume.setVolume(vol)`
Change the volume setting

- vol `Number` - Integer to set volume to

#### `volume.increaseVolume(amount)`
Raise the volume by an amount

- amount `Number` - Optional number to raise volume by
    - For example, if volume is 50 and amount is 5, then the volume will change to 55
    - If we exceed 100 when adding new values, volume will stop at 100
    - By default, this is 5

#### `volume.decreaseVolume(amount)`
Lower the volume by an amount

- amount `Number` - Optional number to lower volume by
    - For example, if volume is 50 and amount is 5, then the volume will change to 45
    - If we exceed 0 when subtracting new values, volume will stop at 0
    - By default, this is 5

### Playback
`gmusic.playback` exposes interfaces to the state of music playback and its behavior (e.g. shuffle).

#### `playback.getPlaybackTime()`
Retrieve the current progress in a song

**Returns:**

- retVal `Number` - Integer representing milliseconds from the start of the song

#### `playback.setPlaybackTime(milliseconds)`
Jump the current song to a time

- milliseconds `Number` - Integer representing milliseconds to jump the current track to

#### `playback.getSongInfo()`
Retrieve current song's metadata

**Returns:**

- song `Object` - Container for song info
    - title `String` - Name of the song
    - artist `String` - Artist of the song
    - album `String` - Album of the song
    - art `String` - URL for album art of the song
    - duration `Number` - Milliseconds that the track will last for

#### `playback.playPause()`
Toggle between play and pause for the current song

**This will not work if there are no songs in the queue.**

### `playback.getPlaybackState()`

**Returns:**

- retVal `Number` - Current status of music playback (e.g. 0, 1, 2)
    - 0 - Playback is stopped
    - 1 - Song is paused
    - 2 - Song is playing
    - Values are available via `GMusic.Playback.STOPPED`, `GMusic.Playback.PAUSED`, and `GMusic.Playback.PLAYING`

#### `playback.forward()`
Move to the next song

#### `playback.rewind()`
Move to the previous song

#### `playback.getShuffle()`
Retrieve the status of shuffle

**Returns:**

- retVal `String` - Current state of shuffle (e.g. `ALL_SHUFFLE`, `NO_SHUFFLE`)
    - `ALL_SHUFFLE` will shuffle between all tracks
    - `NO_SHUFFLE` will play the tracks in the order they were added
    - We created constants named `GMusic.Playback.ALL_SHUFFLE` or `GMusic.Playback.NO_SHUFFLE`

#### `playback.setShuffle(mode)`
Set the shuffle mode

- mode `String` - Value to change shuffle to
  - Valid values are `ALL_SHUFFLE` and `NO_SHUFFLE`

#### `playback.toggleShuffle()`
Toggle to between shuffle being active or inactive

#### `playback.getRepeat()`
Retrieve the current setting for repeat

**Returns:**

- retVal `String` - Current setting for repeat (e.g. `LIST_REPEAT`, `SINGLE_REPEAT`, `NO_REPEAT`)
    - `LIST_REPEAT` will repeat the queue when it reaches the last song
    - `SINGLE_REPEAT` will repeat the current song indefinitely
    - `NO_REPEAT` will not repeat the queue
    - We created constants named `GMusic.Playback.LIST_REPEAT`, `GMusic.Playback.SINGLE_REPEAT`, `GMusic.Playback.NO_REPEAT`

#### `playback.setRepeat(mode)`
Change the current setting for repeat

- mode `String` - Value to change repeat to
    - Valid values are `NO_REPEAT`, `LIST_REPEAT`, `SINGLE_REPEAT`
        - See `playback.getRepeat()` for meaning

#### `playback.toggleRepeat()`
Toggle through the modes for repeat.

- The order is `NO_REPEAT`, `LIST_REPEAT`, `SINGLE_REPEAT`

#### `playback.toggleVisualization()`
Trigger a visualization for the track. This is typically album art.

**This is an untested method.**

### Rating
`gmusic.rating` exposes interfaces to the rating the current song.

#### `rating.getRating()`
Retrieve the rating for the current track.

**Returns:**

- retVal `String` - Rating for current song. This varies from 0 to 5
    - If 0, then there has been no rating
    - On a thumbs system, thumbs down is 1 and thumbs up is 5

#### `rating.toggleThumbsUp()`
Switch between thumbs up and no thumbs up for the current track. If thumbs down was set, this will remove the thumbs down rating.

#### `rating.toggleThumbsDown()`
Switch between thumbs down and no thumbs down for the current track. If thumbs up was set, this will remove the thumbs up rating.

#### `rating.setRating(rating)`
Set the rating for the current track

- rating `String` - Rating to set for the current track. This should be between 1 and 5

#### `rating.resetRating()`
Removes existing rating from the current track

### Extras
`gmusic.extras` is a collection of utility functions for Google Music

#### `extras.getSongURL()`
Retrieve the URL of the current song for sharing

**This is an untested method**

**Returns:**

- retVal `String` - URL for current song

### Hooks
Hooks are currently bound via `.on` and other corresponding methods for [node's EventEmitter][EventEmitter]

[EventEmitter]: http://nodejs.org/api/events.html

```js
gmusic.on('change:song', function (song) {
});
```

#### `.on('change:song')`
Triggers when a song changes

```js
gmusic.on('change:song', function (song) {
});
```

- song `Object` - Same as return value of `playback.getSongInfo()`

#### `.on('change:shuffle')`
Triggers when shuffle is toggled

```js
gmusic.on('change:shuffle', function (mode) {
});
```

- mode `String` - Mode that shuffle changed to
    - Values are consistent with `playback.getShuffle()`

#### `.on('change:repeat')`
Triggers when repeat is toggled

```js
gmusic.on('change:repeat', function (mode) {
});
```

- mode `String` - Mode that repeat changed to
    - Values are consistent with `playback.getRepeat()`

#### `.on('change:playback')`
Triggers when a song is started, paused, or stopped

```js
gmusic.on('change:playback', function (mode) {
});
```

- mode `String` - Same as return value of `playback.getPlaybackState()`

#### `.on('change:playback-time')`
Triggers when playback shifts

```js
gmusic.on('change:playback-time', function (playbackInfo) {
});
```

- playbackInfo `Object` - Container for playback info
    - current `Number` - Milliseconds of how far a track has progressed
    - total `Number` - Milliseconds of how long a track is

#### `.on('change:rating')`
Triggers when the current song is rated

```js
gmusic.on('change:rating', function (rating) {
});
```

- rating `Number` - Rating the current song changed to
    - Consistent with values provided by `rating.getRating()`

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint via `npm run lint` and test via `npm test`.

### Testing
Currently, we require a personal Google account exclusively for testing. We will be rating tracks, changing repeat settings, and need predictable track titles. We are using the following songs (at least 3 required):

> Credentials: musopen@mt2014.com / password

https://musopen.org/music/1333/wolfgang-amadeus-mozart/the-marriage-of-figaro-k-492/

> Music cannot be uploaded via webdriver instance nor incognito window
>
> If you don't want to contaminate your personal account, create a new user profile in Chrome.

For exactly one track, set the following via "Edit Info" in Google Music:

```
Name:
this-is-a-name

Artist:
this-is-an-artist

Album Artist:
this-is-an-album-artist

Album:
this-is-an-album

Composer:
this-is-a-composer

Genre:
this-is-a-genre

Year:
2000

Track #:
1 of 10

Disc #:
3 of 5

Explicit:
Unchecked
```

Once your Google account is registered and the music is uploaded, set the following environment variables

Once your Google account is registered and the music is uploaded, we are ready to run our test suite:

```bash
# Set up Google Music credentials as environment variables
export GOOGLE_MUSIC_JS_EMAIL="your.google@account.email"
export GOOGLE_MUSIC_JS_PASSWORD="your-google-account-password"
# If you are on Windows, use the following
# set GOOGLE_MUSIC_JS_EMAIL="your.google@account.email"
# set GOOGLE_MUSIC_JS_PASSWORD="your-google-account-password"

# Install Selenium server files
npm run webdriver-manager-update

# Start up a Selenium server
npm run webdriver-manager-start &

# Run our tests
npm test
# If you are on Windows, please use
# npm run test-windows
```

##### Attribution
We have an automatic build setup on Travis CI that will run every day to catch changes made by Google as quick as possible. These tests make use of BrowserStack's services to remotely run Selenium tests, huge thanks to [BrowserStack][] for the support of Open Source projects like this one!  

[BrowserStack]: https://www.browserstack.com/

<img src="https://cdn.rawgit.com/gilbarbara/logos/081a89a93b6af2bba5cf4eeace7a5c430785c003/logos/browserstack.svg" height="100px" alt="BrowserStack Logo">

##### Nightlies
Nightly builds are triggered on Travis CI by a third party service service [Nightli.es][]

[Nightli.es]: http://nightli.es/

#### Debugging
Finnicky tests can be debugged by a few methods

- Use `describe.only` and `it.only` to limit test suite to only run 1 at a time
    -
- Use the following script to control

```
# Enter into the node REPL
node

// Prevent node from exiting via anything but an interrupt
process.exit = function () {};

// Emulate mocha being invoked normally
process.argv = ['node', '_mocha', '--timeout', '10000'];
require('mocha/bin/_mocha');

// Our test suite has written `global.browser` as the last Selenium session
global.browser;
browser;
```

## Donating
Support this project and [others by twolfson][gratipay] via [gratipay][].

[![Support via Gratipay][gratipay-badge]][gratipay]

[gratipay-badge]: https://cdn.rawgit.com/gratipay/gratipay-badge/2.x.x/dist/gratipay.png
[gratipay]: https://www.gratipay.com/twolfson/

## License
All files were originally licensed at `5ccfa7b3c7bc5231284f8e42c6a2f2e7fe1e1532` under the MIT license. This can be viewed its [`LICENSE.md`][]. It has been renamed to [LICENSE-MIT][] for ease of disambiguity.

[`LICENSE.md`]: https://github.com/gmusic-utils/gmusic.js/blob/5ccfa7b3c7bc5231284f8e42c6a2f2e7fe1e1532/LICENSE.md
[LICENSE-MIT]: LICENSE-MIT

After this commit, all alterations made by Todd Wolfson and future contributors are released to the Public Domain under the [UNLICENSE][].

[UNLICENSE]: UNLICENSE
