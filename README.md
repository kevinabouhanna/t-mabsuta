Creepyface is a little JavaScript library that makes your face look at the pointer (or [dance 💃](packages/creepyface-dance)).

## Creepyface Usage

```html
<script src="https://creepyface.io/creepyface.js"></script>

<img
  data-creepyface
  src="https://creepyface.io/img/nala/serious"
  data-src-hover="https://creepyface.io/img/nala/hover"
  data-src-look-0="https://creepyface.io/img/nala/0"
  data-src-look-45="https://creepyface.io/img/nala/45"
  data-src-look-90="https://creepyface.io/img/nala/90"
  data-src-look-135="https://creepyface.io/img/nala/135"
  data-src-look-180="https://creepyface.io/img/nala/180"
  data-src-look-225="https://creepyface.io/img/nala/225"
  data-src-look-270="https://creepyface.io/img/nala/270"
  data-src-look-315="https://creepyface.io/img/nala/315"
/>
```

> Run this example on [codepen](https://codepen.io/4lejandrito/pen/vbgxEB).

Creepyface will automatically detect your image (thanks to the `data-creepyface` attribute) and make it look at the mouse or fingers depending on which device you are using.

You can add as many Creepyfaces as you want as long as they all have the `data-creepyface` attribute.

If you want to stop Creepyface on a given image:

```js
creepyface.cancel(document.querySelector('img'))
```

### Full list of data attributes

| Name                    | Description                                                                                                                                                                                                         |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `data-creepyface`       | Add this to automatically attach creepyface to your image when the page loads.                                                                                                                                      |
| `data-src-hover`        | The URL of the image to use when the pointer is over your image.                                                                                                                                                    |
| `data-src-look-<angle>` | The URL of the image to use when the pointer forms the specified angle (in degrees) with the center of your image. Add as many as you want.                                                                         |
| `data-timetodefault`    | The amount of time (in milliseconds) after which the default src is restored if no pointer events are received. 1 second by default. 0 means it will never be restored (the image will always look at the pointer). |
| `data-fieldofvision`    | The angle (in degrees) inside which the pointer will be detected by a given direction. 150 by default.                                                                                                              |
| `data-points`           | Optionally, a comma-separated list of point provider names to make your face look at things other than the pointer. See [Super advanced usage](#super-advanced-usage) for more information.                         |

## Advanced usage

For more advanced use cases Creepyface can also be set up via a programmatic API:

```html
<img src="https://creepyface.io/img/nala/serious" />
```

```js
import creepyface from 'creepyface'

const img = document.querySelector('img')

const cancel = creepyface(img, {
  // Image URL to display on hover
  hover: 'https://creepyface.io/img/nala/hover',
  // Each of the images looking at a given direction
  looks: [
    { angle: 0, src: 'https://creepyface.io/img/nala/0' },
    { angle: 45, src: 'https://creepyface.io/img/nala/45' },
    { angle: 90, src: 'https://creepyface.io/img/nala/90' },
    { angle: 135, src: 'https://creepyface.io/img/nala/135' },
    { angle: 180, src: 'https://creepyface.io/img/nala/180' },
    { angle: 225, src: 'https://creepyface.io/img/nala/225' },
    { angle: 270, src: 'https://creepyface.io/img/nala/270' },
    { angle: 315, src: 'https://creepyface.io/img/nala/315' }
  ],
  // Time (in ms) to restore the default image after the last input
  timeToDefault: 1000
  // The angle (in degrees) inside which the pointer will be detected
  fieldOfVision: 150
})

// at some point restore the original image and stop creepyface
cancel()
```

> Run this example on [codepen](https://codepen.io/4lejandrito/pen/bGdBqzX).

## Super advanced usage

Creepyface will look at the pointer by default, however custom point providers can be defined.

For example, to make your face look at a random point every half a second you need to register a [point provider](packages/creepyface/src/types.d.ts#L5-L8):

```js
import creepyface from 'creepyface'

creepyface.registerPointProvider('random', (consumer) => {
  const interval = setInterval(
    () =>
      consumer([
        Math.random() * window.innerWidth,
        Math.random() * window.innerHeight,
      ]),
    500
  )
  return () => {
    clearInterval(interval)
  }
})
```

and consume it using the `data-points` attribute:

```html
<img
  data-creepyface
  data-points="random"
  src="https://creepyface.io/img/nala/serious"
  data-src-hover="https://creepyface.io/img/nala/hover"
  data-src-look-0="https://creepyface.io/img/nala/0"
  data-src-look-45="https://creepyface.io/img/nala/45"
  data-src-look-90="https://creepyface.io/img/nala/90"
  data-src-look-135="https://creepyface.io/img/nala/135"
  data-src-look-180="https://creepyface.io/img/nala/180"
  data-src-look-225="https://creepyface.io/img/nala/225"
  data-src-look-270="https://creepyface.io/img/nala/270"
  data-src-look-315="https://creepyface.io/img/nala/315"
/>
```

> Run this example on [codepen](https://codepen.io/4lejandrito/pen/ZEYJLrN).

or pass it programmatically:

```html
<img src="https://creepyface.io/img/nala/serious" />
```

```js
const img = document.querySelector('img')

creepyface(img, {
  points: 'random',
  hover: 'https://creepyface.io/img/nala/hover',
  looks: [
    { angle: 0, src: 'https://creepyface.io/img/nala/0' },
    { angle: 45, src: 'https://creepyface.io/img/nala/45' },
    { angle: 90, src: 'https://creepyface.io/img/nala/90' },
    { angle: 135, src: 'https://creepyface.io/img/nala/135' },
    { angle: 180, src: 'https://creepyface.io/img/nala/180' },
    { angle: 225, src: 'https://creepyface.io/img/nala/225' },
    { angle: 270, src: 'https://creepyface.io/img/nala/270' },
    { angle: 315, src: 'https://creepyface.io/img/nala/315' },
  ],
})
```

**Note:** several point providers can work at the same time by using a comma-separated string like `"random,pointer"`.

The following point providers are available out of the box:

- `pointer` for both mouse and touch events. This is the default.
- `mouse` just for mouse events.
- `finger` just for touch events.

There are also external point providers:

- 💃 [dance](packages/creepyface-dance) to dance to the rythm of the music.
- 🤳 [tilt](packages/creepyface-tilt) to stare at you when you tilt your phone.
- 🐝 [firefly](packages/creepyface-firefly) to follow a moving firefly on the screen.

## Developing

- `yarn && yarn build` will set up the packages (using workspaces and [Lerna](https://lerna.js.org/)) and run a required initial build.
- `yarn dev` will spin up local servers for each of the packages.
- `yarn test` will run the tests.
