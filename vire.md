# Vire specific changes

## Issues

### Mobile mp4 video

The `webvr-polyfill` version `dmarcos/webvr-polyfill#a02a8089b` used
in aframe `0.5.0` is an old version that causes a weird issue on some
mobile devices (like the Samsung Galaxy S6). These devices stop
playing mp4 videos, which are used as textures ('a-video' or
'a-videosphere') as soon as the VR mode is active. After the issue
appears it is often not possible to play any mp4 videos anymore until
the device is restarted. The `webvr-polyfill` contains a so-called
wakelock that keeps the mobile device awake, when it is not receiving
any touch events (when it is in a cardboard for example). This
wakelook uses a video to do the trick. My guess it that browser don't
let the device fell asleep, when some video is playing. However the
old version of the `webvr-polyfill` contains some mp4 video data (as
base64) for the wakelock, that causes the problem on some mobile
devices as described above.

The first attempt to solve the issue was to replace the video data in
`vendor/wakelock/wakelock.js` with the mp4 data from a recent
`webvr-polyfill` version (`0.9.34`). But the aframe build process
doesn't seem to do something with `vendor` folder.

Therefore the version of the "webvr-polyfill" was switched to
"^0.9.34" in the `package.json` (same version that is currently used
by the aframe master). This second attempt fixed the problem.