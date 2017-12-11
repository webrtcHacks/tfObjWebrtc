# Tensorflow Object Detection API Web Service

This is an example of how to turn the [TensorFlow Object API](https://github.com/tensorflow/models/tree/master/research/object_detection) into a web service. 
A Python Flask web server is used to interact with a JavaScript a client library. 
The example shows how you can extract frames from WebRTC's getUserMedia, upload them to the API, and then use the canvas to display them.
This allows use of the TensorFlow Object API on any HTML `<video>` element.

Please see the [Computer Vision on the Web with WebRTC and TensorFlow](https://webrtchacks.com/webrtc-cv-tensorflow/) post on 
[webrtcHacks](https://webrtchacks.com) for a walkthrough and more details.

Example image:
![Detecting Cats](https://webrtchacks.com/wp-content/uploads/2017/12/intro-graphic-3.png)

TensorFlow Object Detection API on a WebRTC getUserMedia stream demo video:

[![TensorFlow Object Detection API on a video element](https://img.youtube.com/vi/vzTXW0hGINM/0.jpg)](https://www.youtube.com/watch?v=vzTXW0hGINM)


TensorFlow Object Detection API on a video element demo video:

[![TensorFlow Object Detection API on a video element](https://img.youtube.com/vi/rNkb9vlW2QY/0.jpg)](https://www.youtube.com/watch?v=rNkb9vlW2QY)


## Quick start with Docker
```$xslt
docker run -it -p 5000:5000 chadhart/tensorflow-object-detection:runserver
```

## Installation
### Docker Install

```$xslt
git clone https://github.com/webrtcHacks/tfObjWebrtc.git
cd tfObjWebrtc
docker run -it -p 5000:5000 --name tf-webrtchacks -v $(pwd):/code chadhart/tensorflow-object-detection:webrtchacks
python setup.py install
python server.py
```

### Manual install

Follow the TensorFlow Object API install [instructions](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/installation.md).
Then run the the instructions above.

## Example web apps

Point your browser to:
-  `https://localhost:5000/local` - shows a mirrored video from a webcam
- `https://localhost:5000/video` - shows object detection running on a HTML `<video>` tag

## Browser support

WebRTC browsers have secure origin restrictions: 
- Chrome will only work on `localhost` unless you add TLS certificates to your server
- Firefox will work on any domain as long as you allow it
- Safari will work, but you will need to "Allow Media Capture on Insecure Domains" 

These should all work fine with any other video source.


Edge is currently not supported (polyfill for `canvas.toBlob` needed)

See the webrtcHacks [link](https://webrtchacks.com/webrtc-cv-tensorflow/) for details.

## API Details

Point to a `<script>` tag to `objDetect.js` with an id of `objDetect`. Include `data-source="myVideo"` and other optional `data-` tags to set parameters.

Example:
```$xslt
<script id="objDetect" src="/static/objDetect.js" data-source="myVideo" data-mirror="true" data-uploadWidth="1280" data-scoreThreshold="0.40"></script>
```

Data tags:

- `data-source` - the ID of the source `<video>` to use. Must be specified.
- `data-uploadWidth` - the width of the upload file. Height will automatically be calculated based on the source video's aspect ratio. Default is `640`.
- `data-mirror` - mirror the boundary boxes. Used is the image is mirrored (as is usual with a local getUserMedia view). Default is `false`.
- `data-scoreThreshold` - only show objects above this confidence threshold. Default is `0.5`
 - `data-apiServer` - the full URL of the TensorFlow Object Detection Web API server location. Default is `/image` off of the current domain - 
 i.e. `http://localhost:5000/image`


