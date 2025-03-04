---
title: MediaRecorder
slug: Web/API/MediaRecorder
---
{{APIRef("Media Recorder API")}}

**`MediaRecorder`** 是 [MediaStream Recording API](/zh-CN/docs/Web/API/MediaStream_Recording_API) 提供的用来进行媒体轻松录制的接口，他需要通过调用 {{domxref("MediaRecorder.MediaRecorder", "MediaRecorder()")}} 构造方法进行实例化。

## 构造函数

- {{domxref("MediaRecorder.MediaRecorder()")}}
  - : 创建一个新的`MediaRecorder`对象，对指定的{{domxref("MediaStream")}} 对象进行录制，支持的配置项包括设置容器的 MIME 类型 (例如`"video/webm"` 或者 `"video/mp4"`) 和音频及视频的码率或者二者同用一个码率

## 配置项

- {{domxref("MediaRecorder.mimeType")}} {{readonlyInline}}
  - : 返回 `MediaRecorder` 对象创建时选择器选择的录制容器的 MIME type
- {{domxref("MediaRecorder.state")}} {{readonlyInline}}
  - : 返回录制对象`MediaRecorder` 的当前状态 (闲置中，录制中或者暂停 ) (`inactive`, `recording`, or `paused`.)
- {{domxref("MediaRecorder.stream")}} {{readonlyInline}}
  - : 返回录制器对象 `MediaRecorder `创建时构造函数传入的 stream 对象
- {{domxref("MediaRecorder.ignoreMutedMedia")}}
  - : 用以指定 `MediaRecorder`是否录制无声的输入源。如果这个属性是 false. 录制器对象`MediaRecorder` 会录制无声的音频或者黑屏的视频，默认值是 false
- {{domxref("MediaRecorder.videoBitsPerSecond")}} {{readonlyInline}}
  - : 返回视频采用的编码比率。它可能和构造函数的设置比率不同. (if it was provided).
- {{domxref("MediaRecorder.audioBitsPerSecond")}} {{readonlyInline}}
  - : 返回音频采用的编码比率，它可能和构造函数中设置的比率不同. (if it was provided).

## 方法

- {{domxref("MediaRecorder.canRecordMimeType()", "MediaRecorder.isTypeSupported()")}}
  - : 返回一个{{domxref("Boolean")}} 值，来表示设置的 MIME type 是否被当前用户的设备支持。
- {{domxref("MediaRecorder.pause()")}}
  - : 暂停媒体录制
- {{domxref("MediaRecorder.requestData()")}}
  - : 请求一个从开始到当前接收到的，存储为{{domxref("Blob")}}类型的录制内容. (或者是返回从上一次调用`requestData()` 方法之后到现在的内容). 调用这个方法后，录制将会继续进行，但是会创建一个新的`Blob`对象
- {{domxref("MediaRecorder.resume()")}}
  - : 继续录制之前被暂停的录制动作。
- {{domxref("MediaRecorder.start()")}}
  - : 开始录制媒体，这个方法调用时可以通过给`timeslice`参数设置一个毫秒值，如果设置这个毫秒值，那么录制的媒体会按照你设置的值进行分割成一个个单独的区块，而不是以默认的方式录制一个非常大的整块内容。
- {{domxref("MediaRecorder.stop()")}}
  - : 停止录制。同时触发{{event("dataavailable")}}事件，返回一个存储`Blob`内容的录制数据。之后不再记录

## 静态方法

- {{domxref("MediaRecorder.isTypeSupported()")}}
  - : 静态方法，判断给定的 MIME 类型是否支持。返回{{domxref("Boolean")}}类型的值。

## 事件处理

- {{domxref("MediaRecorder.ondataavailable")}}
  - : 调用它用来处理 {{event("dataavailable")}} 事件，该事件可用于获取录制的媒体资源 (在事件的 `data` 属性中会提供一个可用的 {{domxref("Blob")}} 对象.)
- {{domxref("MediaRecorder.onerror")}}
  - : An {{event("Event_handlers", "event handler")}} called to handle the {{event("recordingerror")}} event, including reporting errors that arise with media recording. These are fatal errors that stop recording.
- {{domxref("MediaRecorder.onpause")}}
  - : 用来处理 {{event("pause")}} 事件，该事件在媒体暂停录制时触发（{{domxref("MediaRecorder.pause()")}}）.
- {{domxref("MediaRecorder.onresume")}}
  - : 用来处理 {{event("resume")}} 事件，该事件在暂停后回复录制视频时触发（{{domxref("MediaRecorder.resume()")}}）.
- {{domxref("MediaRecorder.onstart")}}
  - : 用来处理 {{event("start")}} 事件，该事件在媒体开始录制时触发（{{domxref("MediaRecorder.start()")}}）.
- {{domxref("MediaRecorder.onstop")}}
  - : 用来处理 {{event("stop")}} 事件，该事件会在媒体录制结束时、媒体流（{{domxref("MediaStream")}}）结束时、或者调用{{domxref("MediaRecorder.stop()")}} 方法后触发。

## 事件

Listen to these events using `addEventListener()` or by assigning an event listener to the `oneventname` property of this interface.

- [`error`](/zh-CN/docs/Web/API/MediaRecorder/error_event)
  - : Fired when an error occurs: for example because recording wasn't allowed or was attempted using an unsupported codec.
    Also available via the [`onerror`](/zh-CN/docs/Web/API/MediaRecorder/onerror) property.

## 例子

```
if (navigator.mediaDevices) {
  console.log('getUserMedia supported.');

  var constraints = { audio: true };
  var chunks = [];

  navigator.mediaDevices.getUserMedia(constraints)
  .then(function(stream) {

    var mediaRecorder = new MediaRecorder(stream);

    visualize(stream);

    record.onclick = function() {
      mediaRecorder.start();
      console.log(mediaRecorder.state);
      console.log("recorder started");
      record.style.background = "red";
      record.style.color = "black";
    }

    stop.onclick = function() {
      mediaRecorder.stop();
      console.log(mediaRecorder.state);
      console.log("recorder stopped");
      record.style.background = "";
      record.style.color = "";
    }

    mediaRecorder.onstop = function(e) {
      console.log("data available after MediaRecorder.stop() called.");

      var clipName = prompt('Enter a name for your sound clip');

      var clipContainer = document.createElement('article');
      var clipLabel = document.createElement('p');
      var audio = document.createElement('audio');
      var deleteButton = document.createElement('button');

      clipContainer.classList.add('clip');
      audio.setAttribute('controls', '');
      deleteButton.innerHTML = "Delete";
      clipLabel.innerHTML = clipName;

      clipContainer.appendChild(audio);
      clipContainer.appendChild(clipLabel);
      clipContainer.appendChild(deleteButton);
      soundClips.appendChild(clipContainer);

      audio.controls = true;
      var blob = new Blob(chunks, { 'type' : 'audio/ogg; codecs=opus' });
      chunks = [];
      var audioURL = URL.createObjectURL(blob);
      audio.src = audioURL;
      console.log("recorder stopped");

      deleteButton.onclick = function(e) {
        evtTgt = e.target;
        evtTgt.parentNode.parentNode.removeChild(evtTgt.parentNode);
      }
    }

    mediaRecorder.ondataavailable = function(e) {
      chunks.push(e.data);
    }
  })
  .catch(function(err) {
    console.log('The following error occured: ' + err);
  })
}
```

> **备注：** This code sample is inspired by the Web Dictaphone demo. Some lines have been omitted for brevity; [refer to the source](https://github.com/mdn/web-dictaphone/) for the complete code.

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat("api.MediaRecorder")}}

## See also

- [Using the MediaRecorder API](/zh-CN/docs/Web/API/MediaRecorder_API/Using_the_MediaRecorder_API)
- [Web Dictaphone](https://mdn.github.io/web-dictaphone/): MediaRecorder + getUserMedia + Web Audio API visualization demo, by [Chris Mills](https://twitter.com/chrisdavidmills) ([source on Github](https://github.com/mdn/web-dictaphone/).)
- [Recording a media element](/zh-CN/docs/Web/API/MediaStream_Recording_API/Recording_a_media_element)
- [simpl.info MediaStream Recording demo](https://simpl.info/mediarecorder/), by [Sam Dutton](https://twitter.com/sw12).
- {{domxref("navigator.mediaDevices.getUserMedia")}}
- [FingerSpell](https://www.fingerspell.org/): Sign Language Fingerspelling practice using MediaDevices and the MediaStream Recording API to create and download recordings, MediaRecorder API supported desktop browsers only ([source on GitHub](https://github.com/chrisjohndigital/CameraCaptureJS))
