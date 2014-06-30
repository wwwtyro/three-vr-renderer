### three-vr-renderer

VR renderer for THREE.js utilizing the upcoming VR APIs in popular browsers.

#### Overview

_Because Firefox is the sole browser with this support available, this short tutorial will be specific to Firefox._

##### Create the HMD and sensor objects

{% highlight javascript %}
window.addEventListener("load", function() {
    navigator.mozGetVRDevices(vrDeviceCallback);
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
            vrPosDev = vrdevs[i];
            break;
        }
    }
}
{% endhighlight %}

##### Create a VRRenderer

{% highlight javascript %}
renderer = new THREE.WebGLRenderer();
vrrenderer = new THREE.VRRenderer(renderer, vrHMD);
{% endhighlight %}

##### Orient the camera and render

{% highlight javascript %}
var state = vrPosDev.getState();
camera.quaternion.set(state.orientation.x, 
                      state.orientation.y, 
                      state.orientation.z, 
                      state.orientation.w);
vrrenderer.render(scene, camera);
{% endhighlight %}
