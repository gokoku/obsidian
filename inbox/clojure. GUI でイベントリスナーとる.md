---
type: note
---

#clojure

---
2023-05-20  21:25

# clojure. GUI でイベントリスナーとる

```clojure
(ns myEvent.core
  (:import [java.awt.event KeyEvnet]
           [java.awt JFrame]))

(deftype MyKeyListener []
  KeyListener
  (keyTyped [this e]
    (println "Key typed: " (.getKeyChar e)))
  (keyPressed [this e]
    (println "Key pressed: " (.getKeyCode e)))
  (keyReleased [this e]
    (println "Key released: " (.getKeyCode e))))

(let [frame (JFrame. "Key Event Example")
	  listener (MyKeyListener.)]
  (.addKeyLIstener frame listener)
  (.setSize frame 300 200)
  (.setVisible frame true))
```


![[Pasted image 20230520213041.png]]

reple でキーイベントが表示される。

このフォームは java の形なのだろうか。
では、java ではどう書くのだろう。

# java

```java
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import javax.swing.JFrame;

public class myKeyEvent {
  public static void main(String[] args) {
    
    JFrame frame = new JFrame(title: "Key Event Example");
    
    KeyListener listener = new KeyListener() {
      @Override
 .    public void keyTyped (KeyEvent e) {
	      System.out.println("Key typed: " + e.getKeyChar());
      }
      @Override
      public void keyPressed(KeyEvent e) {
        System.out.println("Key pressed: " + e.getKeyCode());
      }
      @Override
      public void keyReleased(KeyEvent e) {
        System.out.println("Key released: " + e.getKeyCode());
      }
    }

    frame.addKeyListener(listener);
    frame.setSize(width:300, height:200);
    frame.setVisible(b:true);
  }
}
```
