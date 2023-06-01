#android

---
2021-10-07

# Getting Started with CameraX

https://developer.android.com/codelabs/camerax-getting-started#0

https://qiita.com/emusute1212/items/6195cf18bfcbea2ef1d1

CameraX と TensorFlow との組み合わせ

https://qiita.com/SY-BETA/items/fc661a72d08f7f59cf41

Camera , Camera2 を使いやすくして 5.0 でも使えるようにしたライブラリとのこと。

Empty Activity でプロジェクトを作った。

Project : Camerax-sample

### Create the project


app/build.gradle  (Module: Camerax-sample) に追記する。

```json
dependencies {
  def camerax_version = "1.0.1"  
  implementation "androidx.camera:camera-camera2:$camerax_version"  
  implementation "androidx.camera:camera-lifecycle:$camerax_version"  
  implementation "androidx.camera:camera-view:1.0.0-alpha27"
}
```

```json
plugins {  
 id 'com.android.application'  
 id 'kotlin-android'  
 id 'kotlin-android-extensions'  
}
```

kotlin-android-extensions が必要だった。

同期が始まって、Build SUCCESSFUL

res/layout/activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>  
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"  
 xmlns:app="http://schemas.android.com/apk/res-auto"  
 xmlns:tools="http://schemas.android.com/tools"  
 android:layout_width="match_parent"  
 android:layout_height="match_parent"  
 tools:context=".MainActivity">  
  
 <Button android:id="@+id/camera_capture_button"  
 android:layout_width="100dp"  
 android:layout_height="100dp"  
 android:layout_marginBottom="50dp"  
 android:scaleType="fitCenter"  
 android:text="Take Photo"  
 app:layout_constraintLeft_toLeftOf="parent"  
 app:layout_constraintRight_toRightOf="parent"  
 app:layout_constraintBottom_toBottomOf="parent"  
 android:elevation="2dp" />  
  
 <androidx.camera.view.PreviewView android:id="@+id/viewFinder"  
 android:layout_width="match_parent"  
 android:layout_height="match_parent" />  
  
  
</androidx.constraintlayout.widget.ConstraintLayout>
```

manifests/AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>  
<manifest xmlns:android="http://schemas.android.com/apk/res/android"  
 package="jp.co.people.camerax_sample">  
 <uses-feature android:name="android.hardware.camera.any" />  
 <uses-permission android:name="android.permission.CAMERA" />  
 <application android:allowBackup="true"  
 android:icon="@mipmap/ic_launcher"  
 android:label="@string/app_name"  
 android:roundIcon="@mipmap/ic_launcher_round"  
 android:supportsRtl="true"  
 android:theme="@style/Theme.Cameraxsample">  
  
 <activity android:name=".MainActivity">  
 <intent-filter> <action android:name="android.intent.action.MAIN" />  
  
 <category android:name="android.intent.category.LAUNCHER" />  
 </intent-filter> </activity> </application>  
</manifest>
```


MainActivity.kt

```kotlin
package jp.co.people.camerax_sample  
  
import android.Manifest  
import android.content.pm.PackageManager  
import android.net.Uri  
import android.os.Bundle  
import android.util.Log  
import android.widget.Toast  
import androidx.appcompat.app.AppCompatActivity  
import androidx.camera.core.*  
import androidx.camera.lifecycle.ProcessCameraProvider  
import androidx.core.app.ActivityCompat  
import androidx.core.content.ContextCompat  
import kotlinx.android.synthetic.main.activity_main.*  
import java.io.File  
import java.text.SimpleDateFormat  
import java.util.*  
import java.util.concurrent.ExecutorService  
import java.util.concurrent.Executors  
  
  
class MainActivity : AppCompatActivity() {  
    private var imageCapture: ImageCapture? = null  
  
 private lateinit var outputDirectory: File  
    private lateinit var cameraExecutor: ExecutorService  
  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        setContentView(R.layout.activity_main)  
  
        // Request camera permissions  
 if (allPermissionsGranted()) {  
            startCamera()  
        } else {  
            ActivityCompat.requestPermissions(this, REQUIRED_PERMISSIONS, REQUEST_CODE_PEMISSIONS)  
        }  
        // Set up the listener for take photo button  
        camera_capture_button.setOnClickListener { takePhoto() }  
  
        outputDirectory = getOutputDirectory()  
  
        cameraExecutor = Executors.newSingleThreadExecutor()  
    }  
    override fun onRequestPermissionsResult(requestCode: Int, permissions: Array<String>, grantResults: IntArray) {  
        super.onRequestPermissionsResult(requestCode, permissions, grantResults)  
        if (requestCode == REQUEST_CODE_PEMISSIONS) {  
            if (allPermissionsGranted()) {  
                startCamera()  
            } else {  
                Toast.makeText(this, "Permissions not granted by the user.", Toast.LENGTH_SHORT).show()  
                finish()  
            }  
        }  
    }  
  
    private fun takePhoto() {  
       // Get a stable reference of the modifiable image capture use case  
	 val imageCapture = imageCapture ?: return  
	 // Create time-stamped output file to hold the image  
	 val photoFile = File(  
                outputDirectory,  
	 SimpleDateFormat(FILENAME_FORMAT, Locale.US)  
                        .format(System.currentTimeMillis()) + ".jpg"  
	 )  
       // Create output options object which contains file + metadata  
	 val outputOptions = ImageCapture.OutputFileOptions.Builder(photoFile).build()  
  
        // Set up image capture listener, which is triggered after photo has been taken  
	 imageCapture.takePicture(  
                outputOptions, ContextCompat.getMainExecutor(this), object : ImageCapture.OnImageSavedCallback {  
            override fun onError(exc: ImageCaptureException) {  
                        Log.e(TAG, "Photocapture failed: ${exc.message}", exc)  
                    }  
            override fun onImageSaved(output: ImageCapture.OutputFileResults) {  
                val savedUri = Uri.fromFile(photoFile)  
                val msg = "Photo capture succeeded: $savedUri"  
	          Toast.makeText(baseContext, msg, Toast.LENGTH_SHORT).show()  
                Log.d(TAG, msg)  
            }  
        })  
    }  
  
    private fun startCamera() {  
        val cameraProviderFuture = ProcessCameraProvider.getInstance(this)  
  
        cameraProviderFuture.addListener(Runnable {  
	  	// Userd to bind the lifecycle of cameras to the lifecycle owner  
	      val cameraProvider: ProcessCameraProvider = cameraProviderFuture.get()  
            // Preview  
	      val preview = Preview.Builder()  
               .build()  
               .also {  
                  it.setSurfaceProvider(viewFinder.surfaceProvider)  
               }  
	      imageCapture = ImageCapture.Builder().build()  
  
            // Select back camera as a default  
            val cameraSelector = CameraSelector.DEFAULT_BACK_CAMERA  
  
            try {  
                // Unbind use cases before rebinding  
                cameraProvider.unbindAll()  
  
                // Bind use cases to camera  
                cameraProvider.bindToLifecycle(  
                        this, cameraSelector, preview, imageCapture)  
		    } catch(exc: Exception) {  
                	Log.e(TAG, "Use case binding failed", exc)  
	     }  
        }, ContextCompat.getMainExecutor(this))  
    }  
  
    private fun allPermissionsGranted() = REQUIRED_PERMISSIONS.all {  
	 ContextCompat.checkSelfPermission(baseContext, it) == PackageManager.PERMISSION_GRANTED  
    }  
  
    private fun getOutputDirectory(): File {  
        val mediaDir = externalMediaDirs.firstOrNull()?.let {  
        File(it, resources.getString(R.string.app_name)).apply { mkdirs() } }  
        return if (mediaDir != null && mediaDir.exists())  
             mediaDir else filesDir  
    }  
  
    override fun onDestroy() {  
        super.onDestroy()  
        cameraExecutor.shutdown()  
    }  
  
    companion object {  
        private const val TAG = "CameraXBasic"  
        private const val FILENAME_FORMAT = "yyyy-MM-dd-HH-mm-ss-SSS"  
        private const val REQUEST_CODE_PEMISSIONS = 10  
        private val REQUIRED_PERMISSIONS = arrayOf(Manifest.permission.CAMERA)  
    }  
}
```


撮ったファイルをコピーする。

```shell
$ adb shell
> cd /sdcaard/Android/media/jp.co.people.camerax_sample/Camerax-sample
> ls -la

ここにあるのが見て取れた。
```

```shell
$ adb pull /sdcard/Android/media/jp.co.people.camerax_sample/Camerax-sample/2021-10-08-11-52-28-681.jpg

$ open 2021-10-08-11-52-28-681.jpg
```

