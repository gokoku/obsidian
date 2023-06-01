#python

https://ppb.readthedocs.io/en/stable/getting-started.html#prerequisites

```shell
$ mkdir -p path/to/my_game
$ path/to/my_game

$ python3 -m venv .venv
$ source .venv/bin/activate

$ pip install ppb
```

これで、このディレクトリの中で activate した python3 が使われる。
install した ppg は ./venv/lib/python3.9/site-packages に入っている。

### A Basic Game

```python
import ppb

ppb.run()
```

```shell
$ python main.py
```

![](image-kov07ln5.png)

800 x 600 pixels の画面が出た。
真ん中が(0,0) らしい。

```python
import ppb


class Player(ppb.Sprite):
    velocity = ppb.Vector(0, 1)

    def on_update(self, update_event, signal):
        self.position += self.velocity * update_event.time_delta
        
def setup(scene):
    scene.add(Player())
    
ppb.run(setup=setup)
```

箱が上に上がっていった。

### Taking Control

```python
import ppb
from ppb import keycodes
from ppb.events import KeyPressed, KeyReleased


class Player(ppb.Sprite):
    position = ppb.Vector(0, -3)
    direction = ppb.Vector(0, 0)
    speed = 4
    left = keycodes.Left
    right = keycodes.Right

    def on_update(self, update_event, signal):
        self.position += self.direction * self.speed * update_event.time_delta

    def on_key_pressed(self, key_event: KeyPressed, signal):
        if key_event.key == self.left:
            self.direction += ppb.Vector(-1, 0)
        elif key_event.key == self.right:
            self.direction += ppb.Vector(1, 0)
    def on_key_released(self, key_event: KeyReleased, signal):
        if key_event.key == self.left:
            self.direction += ppb.Vector(1, 0)
        elif key_event.key == self.right:
            self.direction += ppb.Vector(-1, 0)
        
        
def setup(scene):
    scene.add(Player())
    
ppb.run(setup=setup)
```

キーイベントは押した瞬間と話した瞬間だけなので、キーを押したら Vector を (0,0) にするためにリリース時に戻してる。


### Reaching Out

Projectile はミサイルとかの発射体のことらしい。

```python
import ppb
from ppb import keycodes
from ppb.events import KeyPressed, KeyReleased

class Projectile(ppb.Sprite):
    size = 0.25
    direction = ppb.Vector(0,1)
    speed = 6

    def on_update(self, update_event, signal):
        if self.direction:
            direction = self.direction.normalize()
        else:
            direction = self.direction
        self.position += direction * self.speed * update_event.time_delta
    


class Player(ppb.Sprite):
    position = ppb.Vector(0, -3)
    direction = ppb.Vector(0, 0)
    speed = 4
    left = keycodes.Left
    right = keycodes.Right
    projector = keycodes.Space

    def on_update(self, update_event, signal):
        self.position += self.direction * self.speed * update_event.time_delta

    def on_key_pressed(self, key_event: KeyPressed, signal):
        if key_event.key == self.left:
            self.direction += ppb.Vector(-1, 0)
        elif key_event.key == self.right:
            self.direction += ppb.Vector(1, 0)
        elif key_event.key == self.projector:
            key_event.scene.add(Projectile(position=self.position + ppb.Vector(0, 0.5)))
            
    def on_key_released(self, key_event: KeyReleased, signal):
        if key_event.key == self.left:
            self.direction += ppb.Vector(1, 0)
        elif key_event.key == self.right:
            self.direction += ppb.Vector(-1, 0)
        
        
def setup(scene):
    scene.add(Player())
    
ppb.run(setup=setup)
```

スペースキーで弾を発射する。

### Something to Target

たま打つならターゲットがほしい。

```python
import ppb
from ppb import keycodes
from ppb.events import KeyPressed, KeyReleased

class Target(ppb.Sprite):
    def on_update(self, update_event, signal):
        for p in update_event.scene.get(kind=Projectile):
            if (p.position - self.position).length <= self.size:
                update_event.scene.remove(self)
                update_event.scene.remove(p)
                break

class Projectile(ppb.Sprite):
    size = 0.25
    direction = ppb.Vector(0,1)
    speed = 6

    def on_update(self, update_event, signal):
        if self.direction:
            direction = self.direction.normalize()
        else:
            direction = self.direction
        self.position += direction * self.speed * update_event.time_delta
    


class Player(ppb.Sprite):
    position = ppb.Vector(0, -3)
    direction = ppb.Vector(0, 0)
    speed = 4
    left = keycodes.Left
    right = keycodes.Right
    projector = keycodes.Space

    def on_update(self, update_event, signal):
        self.position += self.direction * self.speed * update_event.time_delta

    def on_key_pressed(self, key_event: KeyPressed, signal):
        if key_event.key == self.left:
            self.direction += ppb.Vector(-1, 0)
        elif key_event.key == self.right:
            self.direction += ppb.Vector(1, 0)
        elif key_event.key == self.projector:
            key_event.scene.add(Projectile(position=self.position + ppb.Vector(0, 0.5)))
            
    def on_key_released(self, key_event: KeyReleased, signal):
        if key_event.key == self.left:
            self.direction += ppb.Vector(1, 0)
        elif key_event.key == self.right:
            self.direction += ppb.Vector(-1, 0)
        
        
def setup(scene):
    scene.add(Player())

    for x in range(-4, 5, 2):
        scene.add(Target(position=ppb.Vector(x, 3)))
    
ppb.run(setup=setup)

```

![](image-kov1p8ce.png)


# チュートリアルに続く

https://ppb.readthedocs.io/en/stable/tutorials/index.html
