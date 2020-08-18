# Face AR

A framework for augmented faces on the web.

## Usage

Create a basic example:

```html
<canvas id="canvas-id" width="680" height="480"></canvas>

<script src="js/face-ar.js"></script>
<script type="text/javascript">
  // create a built-in filter
  const stereoGlassesFilter = new FaceAR.filters.StereoGlassesFilter()

  const session = new FaceAR.Session({
    apiKey: 'api-key', // API key
    canvasID: 'canvas-id', // HTML canvas element ID
    maximumFaceCount: 1, // maximum number of faces to detect
    filters: [stereoGlassesFilter], // registered filters to preload
    onInit: (options, scene, error) => {
      // check for errors
      if (error) {
        console.error(error.message)
      }
    },
    onUpdate: (options, scene, anchors) => {
      // update filter at each frame
      if (anchors.length > 0) {
        stereoGlassesFilter.update(anchors[0])
      }
    }
  })
</script>
```

Resize session:

```javascript
session.resize(width, height)
```

The [three.js](https://threejs.org) library is accessible by `FaceAR.Three` name and the scenes are instances of the `FaceAR.Three.Scene` object.

Try a [live demo](https://danilopeixoto.github.io/face-ar) of the Face AR sample application.

## Custom filters

Create a custom filter:

```javascript
const customFilter = new FaceAR.Filter({
  url: 'assets/scene.gltf', // GLTF/GLB scene resource or undefined
  onInit: (options, scene, error) => {
    // modify or add object to scene if path is undefined
  },
  onUpdate: (options, scene, anchor) => {
    // update scene at each frame
  }
})
```

Each `FaceAR.Anchor` object represent a detection result defined by the following attributes:

```javascript
{
  score: 1.0, // detection confidence
  box: { // detection bounding box
    minimum: [231.21, 145.25],
    maximum: [469.72, 309.34]
  },
  annotations: [ // semantic groupings of the mesh coordinates
    silhouette: [
      [325.18, 122.75, 5.82],
      ...
    ],
    lipsUpperOuter: ...,
    lipsLowerOuter: ...,
    lipsUpperInner: ...,
    lipsLowerInner: ...,
    rightEyeUpper0: ...,
    rightEyeLower0: ...,
    rightEyeUpper1: ...,
    rightEyeLower1: ...,
    rightEyeUpper2: ...,
    rightEyeLower2: ...,
    rightEyeLower3: ...,
    rightEyebrowUpper: ...,
    rightEyebrowLower: ...,
    leftEyeUpper0: ...,
    leftEyeLower0: ...,
    leftEyeUpper1: ...,
    leftEyeLower1: ...,
    leftEyeUpper2: ...,
    leftEyeLower2: ...,
    leftEyeLower3: ...,
    leftEyebrowUpper: ...,
    leftEyebrowLower: ...,
    midwayBetweenEyes: ...,
    noseTip: ...,
    noseBottom: ...,
    noseRightCorner: ...,
    noseLeftCorner: ...,
    rightCheek: ...,
    leftCheek: ...
  ],
  keypoints: [ // 3D coordinates of each facial landmark
    [322.32, 297.58, -18.53],
    ...
  ]
}
```

Use `anchor.track(region)` to place objects relative to face regions:

```javascript
const transform = anchor.track(FaceAR.regions.All)

object.position.copy(transform.position)
object.rotation.setFromRotationMatrix(transform.rotation)
object.scale.copy(transform.scale)
```

Face regions to track:

```
All
Forehead
RightEyebrow
LeftEyebrow
RightEye
LeftEye
RightEar
LeftEar
NoseBridge
NoseTip
RightCheek
LeftCheek
Mouth
Chin
```

## Limitations

* Multi-face tracking is an experimental feature and should not be used in production.
* Face occlusion feature is currently unavailable.

## Copyright and license

Copyright (c) 2020, Danilo Peixoto. All rights reserved.

Project developed under a [proprietary license](LICENSE.md).
