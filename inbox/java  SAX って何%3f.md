#java

---
2021-11-09

# Sax とは?

DOM と SAX は仲間とのこと。
XMLをを処理

https://www.techscore.com/tech/Java/JavaSE/SAX/1/



```xml
<?xml version="1.0" encoding="euc_jp" ?>
<root>
    <text>Hello SAX!!!</text>
</root>
```

```java
package sax;

import java.io.File;
import java.io.IOException;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import org.xml.sax.Attributes;
import javax.xml.parsers.ParserConfigurationException;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;

public class SaxSample extends DefaultHandler{
    public static void main(String[] args) throws IOException, ParserConfigurationException, SAXException {
        try {
            SAXParserFactory factory = SAXParserFactory.newInstance();
            SAXParser saxParser = factory.newSAXParser();
            saxParser.parse(new File("/Users/george/Desktop/java/sax/sample.xml"), new SaxSample());
        } catch (ParserConfigurationException | SAXException e) {
            e.printStackTrace();
        }
    }

    public void startDocument() {
        System.out.println("Start document");
    }
    public void endDocument() {
        System.out.println("End document");
    }
    public void startElement(String uri, String localName, String qName, Attributes attributes) {
        System.out.println("Start element: " + qName);
    }
    public void endElement(String uri, String localName, String qName) {
        System.out.println("End element: " + qName);
    }
    public void characters(char[] ch, int start, int length) {
        System.out.println("Characters: " + new String(ch, start, length));
    }
}
```

DefaultHandler を継承。

* startDocument() : ドキュメント開始時に実行。
* endDocument():                         終了時f
* startElement() : エレメント開始タグ読み込み時に実行。
	* uri  名前空間のURI
	* localName : ローカル名
	* qName       : 修飾名
	* attributes   : 属性
* characters()     : 要素のテキスト読み込み時に実行。
	* ch  : 文書のデータが入った char配列
	* offset : 配列中のテキストの入る位置
	* length : 配列の長さ
* endElement() :  エレメントの終了タグの読み込み時に実行
	* uiri
	* localName
	* qName  : 修飾子を持つ修飾名
	