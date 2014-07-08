## three-vr-renderer

VR renderer for THREE.js utilizing the upcoming WebVR API in popular browsers.

### Overview

##### Create the HMD and sensor objects

```javascript
window.addEventListener("load", function() {
    if (navigator.getVRDevices) {
        navigator.getVRDevices().then(vrDeviceCallback);
    } else if (navigator.mozGetVRDevices) {
        navigator.mozGetVRDevices(vrDeviceCallback);
    }
}, false);

function vrDeviceCallback(vrdevs) {
    for (var i = 0; i < vrdevs.length; ++i) {
        if (vrdevs[i] instanceof HMDVRDevice) {
            vrHMD = vrdevs[i];
            break;
        }
    }
    for (var i = 0; i < vrdevs.length; ++i) {
        if (vrdevs[i] instanceof PositionSensorVRDevice &&
            vrdevs[i].hardwareUnitId == vrHMD.hardwareUnitId) {
            vrHMDSensor = vrdevs[i];
            break;
        }
    }
    initScene();
    initRenderer();
    render();
}
```

##### Create a VRRenderer

```javascript
var renderer = new THREE.WebGLRenderer();
var vrrenderer = new THREE.VRRenderer(renderer, vrHMD);
```

##### Go Fullscreen

```javascript
window.addEventListener("keypress", function(e) {
    if (e.charCode == 'f'.charCodeAt(0)) {
        if (renderCanvas.mozRequestFullScreen) {
            renderCanvas.mozRequestFullScreen({
                vrDisplay: vrHMD
            });
        } else if (renderCanvas.webkitRequestFullscreen) {
            renderCanvas.webkitRequestFullscreen({
                vrDisplay: vrHMD,
            });
        }
    }
}, false);
```

##### Orient the camera and render

```javascript
    var state = vrHMDSensor.getState();
    camera.quaternion.set(state.orientation.x, 
                          state.orientation.y, 
                          state.orientation.z, 
                          state.orientation.w);
    vrrenderer.render(scene, camera);
```

### Credits

[Vlad Vukicevic](https://twitter.com/vvuk)

[Brandon Jones](https://twitter.com/Tojiro)