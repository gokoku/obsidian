#java #clojure 

---
2021-10-22

# Java コードを Clojure にする例

```java
import java.awt.Toolkit;
import java.awt.datatransfer.Clipboard;
import java.awt.datatransfer.StringSelection;

public class MyClipboard {

  public static void copy(String text) {
    Clipboard clipboard = Toolkit.getDefaultToolkit().getSystemClipboard();
    StringSelection stringSelection = new StringSelection(text);
    clipboard.setContents(stringSelection, null);
  }

  public static void main(String[] args) {
    copy("Hello java");
  }
}
```

```clojure
(import 'java.awt.Toolkit)
(import 'java.awt.datatransfer.StringSelection)
(import 'java.awt.datatransfer.Clipboard)

(defn copy [text]
  (let [clipboard (.getSystemClipboard (Toolkit/getDefaultToolkit))]
  (.setContents clipboard (StringSelection. text) nil)))

  Hello Clojure!
```

(copy "Hello Clojure") を評価すると、コピペで出る!!!

