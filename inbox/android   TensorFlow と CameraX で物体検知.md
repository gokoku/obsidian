#android 

---
2021-10-19

# TensorFlow と CameraX で物体検知

https://qiita.com/SY-BETA/items/fc661a72d08f7f59cf41

ひとまずこれで動いたので、とっとく。


## assets

TensorFlow Lite のAndroid デモから持ってきた。

main/assets/detect.tflite
main/assets/labelmap.txt 


## activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.camera.view.PreviewView
        android:id="@+id/cameraView"
        android:layout_width="400dp"
        android:layout_height="400dp"
        android:layout_marginTop="10dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        />

    <SurfaceView
        android:id="@+id/resultView"
        android:layout_width="400dp"
        android:layout_height="400dp"
        android:layout_marginTop="10dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        />

    <TextView
        android:id="@+id/textview"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="30dp"
        android:textAlignment="center"
        android:textSize="20sp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/resultView" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

## MainActivity.kt

```kotlin
package jp.co.people.camerax_sample

import android.content.res.AssetFileDescriptor
import android.os.Bundle
import android.util.Log
import android.util.Size
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.camera.core.*
import androidx.camera.lifecycle.ProcessCameraProvider
import androidx.core.content.ContextCompat
import kotlinx.android.synthetic.main.activity_main.*
import org.tensorflow.lite.Interpreter
import org.w3c.dom.Text
import java.io.*
import java.nio.ByteBuffer
import java.nio.channels.FileChannel
import java.util.concurrent.ExecutorService
import java.util.concurrent.Executors
import java.util.concurrent.Executors.newSingleThreadExecutor
import java.util.jar.Manifest

class MainActivity : AppCompatActivity() {

    private lateinit var cameraExecutor: ExecutorService
    private lateinit var overlaySurfaceView: OverlaySurfaceView
    private lateinit var textView: TextView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        textView = findViewById<TextView>(R.id.textview)

        overlaySurfaceView = OverlaySurfaceView(resultView)
        cameraExecutor = Executors.newSingleThreadExecutor()
        setupCamera()
    }


    fun setupCamera() {
        val cameraProviderFuture = ProcessCameraProvider.getInstance(this)

        cameraProviderFuture.addListener({
            val cameraProvider: ProcessCameraProvider = cameraProviderFuture.get()
            val preview = Preview.Builder()
                .build()
                .also { it.setSurfaceProvider(cameraView.surfaceProvider) }
            val cameraSelector = CameraSelector.DEFAULT_FRONT_CAMERA

            // TODO 物体検知
            val imageAnalyzer = ImageAnalysis.Builder()
                .setTargetRotation(cameraView.display.rotation)
                .setBackpressureStrategy(ImageAnalysis.STRATEGY_KEEP_ONLY_LATEST)
                .build()
                .also {
                    it.setAnalyzer(
                        cameraExecutor,
                        ObjectDetector(
                            yuvToRgbConverter,
                            interpreter,
                            labels,
                            Size(resultView.width, resultView.height)
                        ) {
                            detectedObjectList ->
                            // TODO 検出結果の表示
                            overlaySurfaceView.draw(detectedObjectList)
                            displayLabel(detectedObjectList)
                        }
                    )
                }

            try {
                cameraProvider.unbindAll()
                cameraProvider.bindToLifecycle(this, cameraSelector, preview, imageAnalyzer)
            } catch (exc: java.lang.Exception) {
                Log.e("ERROR: Camera", "Use case binding failed", exc)
            }
        }, ContextCompat.getMainExecutor(this))
    }

    override fun onDestroy() {
        super.onDestroy()
        cameraExecutor.shutdown()
    }

    companion object {
        private const val MODEL_FILE_NAME = "detect.tflite"
        private const val LABEL_FILE_NAME = "labelmap.txt"
    }

    private fun displayLabel(detectedObjectList: List<DetectionObject>) {
        detectedObjectList.mapIndexed { i, detectionObject ->
            textView.text = detectionObject.label + " " + "%,.2f".format(detectionObject.score * 100) + "%"

        }

    }

    private val yuvToRgbConverter: YuvToRgbConverter by lazy {
        YuvToRgbConverter(this)
    }

    private val interpreter: Interpreter by lazy {
        Interpreter(loadModel())
    }

    private val labels: List<String> by lazy {
        loadLabels()
    }

    private fun loadModel(fileName: String = MainActivity.MODEL_FILE_NAME): ByteBuffer {
        lateinit var modelBuffer: ByteBuffer
        var file: AssetFileDescriptor? = null
        try {
            file = assets.openFd(fileName)
            val inputStream = FileInputStream(file.fileDescriptor)
            val fileChannel = inputStream.channel
            modelBuffer = fileChannel.map(FileChannel.MapMode.READ_ONLY, file.startOffset, file.declaredLength)
        } catch (e: Exception) {
            Toast.makeText(this, "model load error", Toast.LENGTH_SHORT).show()
            finish()
        } finally {
            file?.close()
        }
        return modelBuffer
    }

    private fun loadLabels(fileName: String = MainActivity.LABEL_FILE_NAME): List<String> {
        var labels = listOf<String>()
        var inputStream: InputStream? = null
        try {
            inputStream = assets.open(fileName)
            val reader = BufferedReader(InputStreamReader(inputStream))
            labels = reader.readLines()
        } catch (e: Exception) {
            Toast.makeText(this, "label load error", Toast.LENGTH_SHORT).show()
            finish()
        } finally {
            inputStream?.close()
        }
        return labels
    }

}
```


## ObjectDetector.kt

```kotlin
package jp.co.people.camerax_sample

import android.annotation.SuppressLint
import android.graphics.Bitmap
import android.graphics.RectF
import android.media.Image
import android.util.Size
import androidx.camera.core.ImageAnalysis
import androidx.camera.core.ImageProxy
import androidx.camera.core.internal.YuvToJpegProcessor
import org.tensorflow.lite.DataType
import org.tensorflow.lite.Interpreter
import org.tensorflow.lite.support.common.ops.NormalizeOp
import org.tensorflow.lite.support.image.ImageProcessor
import org.tensorflow.lite.support.image.TensorImage
import org.tensorflow.lite.support.image.ops.ResizeOp
import org.tensorflow.lite.support.image.ops.Rot90Op
import java.text.Normalizer

typealias ObjectDetectorCallback = (image: List<DetectionObject>) -> Unit

class ObjectDetector (
    private val yuvToRgbConverter: YuvToRgbConverter,
    private val interpreter: Interpreter,
    private val labels: List<String>,
    private val resultViewSize: Size,
    private val listener: ObjectDetectorCallback
) : ImageAnalysis.Analyzer {

    companion object {
        // モデル のinputとoutputサイズ
        private const val IMG_SIZE_X = 300
        private const val IMG_SIZE_Y = 300
        private const val MAX_DETECTION_NUM = 10

        // 今回使うtfliteモデルは量子化済みなので、normalize関連は 127.5f ではなく以下の通り
        private const val NORMALIZE_MEAN = 0f
        private const val NORMALIZE_STD = 1f

        // 検出結果のスコアしきい値
        private const val SCORE_THRESHOLD = 0.5f
    }

    private var imageRotationDegrees: Int = 0
    private val tfImageProcessor by lazy {
        ImageProcessor.Builder()
            .add(ResizeOp(IMG_SIZE_X, IMG_SIZE_Y, ResizeOp.ResizeMethod.BILINEAR))
            .add(Rot90Op(-imageRotationDegrees / 90))
            .add(NormalizeOp(NORMALIZE_MEAN, NORMALIZE_STD))
            .build()
    }
    private val tfImageBuffer = TensorImage(DataType.UINT8)

    private val outputBoundingBoxes: Array<Array<FloatArray>> = arrayOf(
        Array(MAX_DETECTION_NUM) {
            FloatArray(4)
        }
    )

    private val outputLabels: Array<FloatArray> = arrayOf(
        FloatArray(MAX_DETECTION_NUM)
    )

    private val outputScores: Array<FloatArray> = arrayOf(
        FloatArray(MAX_DETECTION_NUM)
    )
    private val outputDetectionNum: FloatArray = FloatArray(1)

    private val outputMap = mapOf(
        0 to outputBoundingBoxes,
        1 to outputLabels,
        2 to outputScores,
        3 to outputDetectionNum
    )

    private fun detect(targetImage: Image): List<DetectionObject> {
        val targetBitmap = Bitmap.createBitmap(targetImage.width, targetImage.height, Bitmap.Config.ARGB_8888)
        yuvToRgbConverter.yuvToRgb(targetImage, targetBitmap)
        tfImageBuffer.load(targetBitmap)
        val tensorImage = tfImageProcessor.process(tfImageBuffer)

        interpreter.runForMultipleInputsOutputs(arrayOf(tensorImage.buffer), outputMap)

        val detectedObjectList = arrayListOf<DetectionObject>()
        loop@ for (i in 0 until outputDetectionNum[0].toInt()) {
            val score = outputScores[0][i]
            val label = labels[outputLabels[0][i].toInt()]
            val boundingBox = RectF(
                outputBoundingBoxes[0][i][1] * resultViewSize.width,
                outputBoundingBoxes[0][i][0] * resultViewSize.height,
                outputBoundingBoxes[0][i][3] * resultViewSize.width,
                outputBoundingBoxes[0][i][2] * resultViewSize.height
            )
            if (score >= ObjectDetector.SCORE_THRESHOLD) {
                detectedObjectList.add(
                    DetectionObject(
                        score = score,
                        label = label,
                        boundingBox = boundingBox
                    )
                )
            } else {
                // 検出結果はスコアの高い順にソートされているので、下回ったら終了
                break@loop
            }
        }
        return detectedObjectList.take(4)
    }
    @SuppressLint("UnsafeExperimentalUsageError")
    override fun analyze(image: ImageProxy) {
        if (image.image == null) return
        imageRotationDegrees = image.imageInfo.rotationDegrees
        val detectedObjectList = detect(image.image!!)
        listener(detectedObjectList)
        image.close()
    }
}

/**
 * 検出結果を入れるクラス
 */
data class DetectionObject(
    val score: Float,
    val label: String,
    val boundingBox: RectF
)


```


## OverlaySurfaceView.kt

```kotlin
package jp.co.people.camerax_sample

import android.graphics.*
import android.view.SurfaceHolder
import android.view.SurfaceView

class OverlaySurfaceView(surfaceView: SurfaceView) : SurfaceView(surfaceView.context), SurfaceHolder.Callback {

    init {
        surfaceView.holder.addCallback(this)
        surfaceView.setZOrderOnTop(true)
    }
    private var surfaceHolder = surfaceView.holder
    private val paint = Paint()
    private val pathColorList = listOf(Color.RED, Color.GREEN, Color.CYAN, Color.BLUE)

    override fun surfaceCreated(holder: SurfaceHolder) {
        surfaceHolder.setFormat(PixelFormat.TRANSPARENT)
    }

    override fun surfaceChanged(holder: SurfaceHolder, format: Int, width: Int, height: Int) {
    }

    override fun surfaceDestroyed(holder: SurfaceHolder) {
    }

    fun draw(detectedObjectList: List<DetectionObject>) {
        val canvas: Canvas? = surfaceHolder.lockCanvas()
        canvas?.drawColor(0, PorterDuff.Mode.CLEAR)

        detectedObjectList.mapIndexed { i, detectionObject ->
            paint.apply {
                color = pathColorList[i]
                style = Paint.Style.STROKE
                strokeWidth = 7f
                isAntiAlias = false
            }
            canvas?.drawRect(detectionObject.boundingBox, paint)

            paint.apply {
                style = Paint.Style.FILL
                isAntiAlias = true
                textSize = 50f
            }
            canvas?.drawText(
                detectionObject.label + " " + "%,.2f".format(detectionObject.score * 100) + "%",
                detectionObject.boundingBox.left,
                detectionObject.boundingBox.top - 5f,
                paint
            )
        }
        surfaceHolder.unlockCanvasAndPost(canvas ?: return )
    }
}
```


## Yuv.kt

```kotlin
package jp.co.people.camerax_sample


import android.graphics.ImageFormat
import android.media.Image
import androidx.annotation.IntDef
import java.nio.ByteBuffer

/*
This file is converted from part of https://github.com/gordinmitya/yuv2buf.
Follow the link to find demo app, performance benchmarks and unit tests.
Intro to YUV image formats:
YUV_420_888 - is a generic format that can be represented as I420, YV12, NV21, and NV12.
420 means that for each 4 luminosity pixels we have 2 chroma pixels: U and V.
* I420 format represents an image as Y plane followed by U then followed by V plane
    without chroma channels interleaving.
    For example:
    Y Y Y Y
    Y Y Y Y
    U U V V
* NV21 format represents an image as Y plane followed by V and U interleaved. First V then U.
    For example:
    Y Y Y Y
    Y Y Y Y
    V U V U
* YV12 and NV12 are the same as previous formats but with swapped order of V and U. (U then V)
Visualization of these 4 formats:
https://user-images.githubusercontent.com/9286092/89119601-4f6f8100-d4b8-11ea-9a51-2765f7e513c2.jpg
It's guaranteed that image.getPlanes() always returns planes in order Y U V for YUV_420_888.
https://developer.android.com/reference/android/graphics/ImageFormat#YUV_420_888
Because I420 and NV21 are more widely supported (RenderScript, OpenCV, MNN)
the conversion is done into these formats.
More about each format: https://www.fourcc.org/yuv.php
*/

@kotlin.annotation.Retention(AnnotationRetention.SOURCE)
@IntDef(ImageFormat.NV21, ImageFormat.YUV_420_888)
annotation class YuvType

class YuvByteBuffer(image: Image, dstBuffer: ByteBuffer? = null) {
    @YuvType
    val type: Int
    val buffer: ByteBuffer

    init {
        val wrappedImage = ImageWrapper(image)

        type = if (wrappedImage.u.pixelStride == 1) {
            ImageFormat.YUV_420_888
        } else {
            ImageFormat.NV21
        }
        val size = image.width * image.height * 3 / 2
        buffer = if (
            dstBuffer == null || dstBuffer.capacity() < size ||
            dstBuffer.isReadOnly || !dstBuffer.isDirect
        ) {
            ByteBuffer.allocateDirect(size) }
        else {
            dstBuffer
        }
        buffer.rewind()

        removePadding(wrappedImage)
    }

    // Input buffers are always direct as described in
    // https://developer.android.com/reference/android/media/Image.Plane#commandetBuffer()
    private fun removePadding(image: ImageWrapper) {
        val sizeLuma = image.y.width * image.y.height
        val sizeChroma = image.u.width * image.u.height
        if (image.y.rowStride > image.y.width) {
            removePaddingCompact(image.y, buffer, 0)
        } else {
            buffer.position(0)
            buffer.put(image.y.buffer)
        }
        if (type == ImageFormat.YUV_420_888) {
            if (image.u.rowStride > image.u.width) {
                removePaddingCompact(image.u, buffer, sizeLuma)
                removePaddingCompact(image.v, buffer, sizeLuma + sizeChroma)
            } else {
                buffer.position(sizeLuma)
                buffer.put(image.u.buffer)
                buffer.position(sizeLuma + sizeChroma)
                buffer.put(image.v.buffer)
            }
        } else {
            if (image.u.rowStride > image.u.width * 2) {
                removePaddingNotCompact(image, buffer, sizeLuma)
            } else {
                buffer.position(sizeLuma)
                var uv = image.v.buffer
                val properUVSize = image.v.height * image.v.rowStride - 1
                if (uv.capacity() > properUVSize) {
                    uv = clipBuffer(image.v.buffer, 0, properUVSize)
                }
                buffer.put(uv)
                val lastOne = image.u.buffer[image.u.buffer.capacity() - 1]
                buffer.put(buffer.capacity() - 1, lastOne)
            }
        }
        buffer.rewind()
    }

    private fun removePaddingCompact(
        plane: PlaneWrapper,
        dst: ByteBuffer,
        offset: Int
    ) {
        require(plane.pixelStride == 1) {
            "use removePaddingCompact with pixelStride == 1"
        }

        val src = plane.buffer
        val rowStride = plane.rowStride
        var row: ByteBuffer
        dst.position(offset)
        for (i in 0 until plane.height) {
            row = clipBuffer(src, i * rowStride, plane.width)
            dst.put(row)
        }
    }

    private fun removePaddingNotCompact(
        image: ImageWrapper,
        dst: ByteBuffer,
        offset: Int
    ) {
        require(image.u.pixelStride == 2) {
            "use removePaddingNotCompact pixelStride == 2"
        }
        val width = image.u.width
        val height = image.u.height
        val rowStride = image.u.rowStride
        var row: ByteBuffer
        dst.position(offset)
        for (i in 0 until height - 1) {
            row = clipBuffer(image.v.buffer, i * rowStride, width * 2)
            dst.put(row)
        }
        row = clipBuffer(image.u.buffer, (height - 1) * rowStride - 1, width * 2)
        dst.put(row)
    }

    private fun clipBuffer(buffer: ByteBuffer, start: Int, size: Int): ByteBuffer {
        val duplicate = buffer.duplicate()
        duplicate.position(start)
        duplicate.limit(start + size)
        return duplicate.slice()
    }

    private class ImageWrapper(image:Image) {
        val width= image.width
        val height = image.height
        val y = PlaneWrapper(width, height, image.planes[0])
        val u = PlaneWrapper(width / 2, height / 2, image.planes[1])
        val v = PlaneWrapper(width / 2, height / 2, image.planes[2])

        // Check this is a supported image format
        // https://developer.android.com/reference/android/graphics/ImageFormat#YUV_420_888
        init {
            require(y.pixelStride == 1) {
                "Pixel stride for Y plane must be 1 but got ${y.pixelStride} instead."
            }
            require(u.pixelStride == v.pixelStride && u.rowStride == v.rowStride) {
                "U and V planes must have the same pixel and row strides " +
                        "but got pixel=${u.pixelStride} row=${u.rowStride} for U " +
                        "and pixel=${v.pixelStride} and row=${v.rowStride} for V"
            }
            require(u.pixelStride == 1 || u.pixelStride == 2) {
                "Supported" + " pixel strides for U and V planes are 1 and 2"
            }
        }
    }

    private class PlaneWrapper(width: Int, height: Int, plane: Image.Plane) {
        val width = width
        val height = height
        val buffer: ByteBuffer = plane.buffer
        val rowStride = plane.rowStride
        val pixelStride = plane.pixelStride
    }
}
```


## YuvToRgbConverter.kt

```kotlin
/*
 * Copyright 2020 The Android Open Source Project
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     https://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package jp.co.people.camerax_sample

import android.content.Context
import android.graphics.Bitmap
import android.graphics.ImageFormat
import android.media.Image
import android.renderscript.Allocation
import android.renderscript.Element
import android.renderscript.RenderScript
import android.renderscript.ScriptIntrinsicYuvToRGB
import android.renderscript.Type
import java.nio.ByteBuffer

/**
 * Helper class used to convert a [Image] object from
 * [ImageFormat.YUV_420_888] format to an RGB [Bitmap] object, it has equivalent
 * functionality to https://github
 * .com/androidx/androidx/blob/androidx-main/camera/camera-core/src/main/java/androidx/camera/core/ImageYuvToRgbConverter.java
 *
 * NOTE: This has been tested in a limited number of devices and is not
 * considered production-ready code. It was created for illustration purposes,
 * since this is not an efficient camera pipeline due to the multiple copies
 * required to convert each frame. For example, this
 * implementation
 * (https://stackoverflow.com/questions/52726002/camera2-captured-picture-conversion-from-yuv-420-888-to-nv21/52740776#52740776)
 * might have better performance.
 */
@Suppress("DEPRECATION")

class YuvToRgbConverter(context: Context) {
    private val rs = RenderScript.create(context)
    private val scriptYuvToRgb =
        ScriptIntrinsicYuvToRGB.create(rs, Element.U8_4(rs))

    // Do not add getters/setters functions to these private variables
    // because yuvToRgb() assume they won't be modified elsewhere
    private var yuvBits: ByteBuffer? = null
    private var bytes: ByteArray = ByteArray(0)
    private var inputAllocation: Allocation? = null
    private var outputAllocation: Allocation? = null

    @Synchronized
    fun yuvToRgb(image: Image, output: Bitmap) {
        val yuvBuffer = YuvByteBuffer(image, yuvBits)
        yuvBits = yuvBuffer.buffer

        if (needCreateAllocations(image, yuvBuffer)) {
            val yuvType = Type.Builder(rs, Element.U8(rs))
                .setX(image.width)
                .setY(image.height)
                .setYuvFormat(yuvBuffer.type)
            inputAllocation = Allocation.createTyped(
                rs,
                yuvType.create(),
                Allocation.USAGE_SCRIPT
            )
            bytes = ByteArray(yuvBuffer.buffer.capacity())
            val rgbaType = Type.Builder(rs, Element.RGBA_8888(rs))
                .setX(image.width)
                .setY(image.height)
            outputAllocation = Allocation.createTyped(
                rs,
                rgbaType.create(),
                Allocation.USAGE_SCRIPT
            )
        }

        yuvBuffer.buffer.get(bytes)
        inputAllocation!!.copyFrom(bytes)

        // Convert NV21 or YUV_420_888 format to RGB
        inputAllocation!!.copyFrom(bytes)
        scriptYuvToRgb.setInput(inputAllocation)
        scriptYuvToRgb.forEach(outputAllocation)
        outputAllocation!!.copyTo(output)
    }

    private fun needCreateAllocations(image: Image, yuvBuffer: YuvByteBuffer): Boolean {
        return (inputAllocation == null ||               // the very 1st call
                inputAllocation!!.type.x != image.width ||   // image size changed
                inputAllocation!!.type.y != image.height ||
                inputAllocation!!.type.yuv != yuvBuffer.type || // image format changed
                bytes.size == yuvBuffer.buffer.capacity())
    }
}
```
