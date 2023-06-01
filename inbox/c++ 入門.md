#cpp

## プロジェクトの作成

```shell
$ projectGenerator my_circle
$ cd my_circle
```

```c++
//--------------------------------------------------------------
void ofApp::setup(){
    ofSetCircleResolution(64);
    ofSetFrameRate(60);
    location = glm::vec2(0, 0);
    velocity = glm::vec2(30, 10);
}

//--------------------------------------------------------------
void ofApp::update(){
    location = location + velocity;
    if (location.x > ofGetWidth() || location.x < 0) {
        velocity.x = velocity.x * -1;
    }
    if (location.y > ofGetHeight() || location.y < 0) {
        velocity.y = velocity.y * -1;
    }

}

//--------------------------------------------------------------
void ofApp::draw(){
    ofSetColor(0, 127, 255, 200);
    ofDrawCircle(location, 20);
    ofSetColor(255);
    ofDrawBitmapString(ofGetFrameRate(), 20, 20);
}

```

```shell
$ make
$ make RunRelease
```


