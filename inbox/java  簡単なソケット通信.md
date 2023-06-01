#java


server
https://qiita.com/akatsubaki/items/aaca7664c5b881415bf5

client
https://qiita.com/akatsubaki/items/ceb935f16c3402d3144d


## Server

```java
import java.io.*;
import java.net.*;
import java.util.*;


public class SampleServer2 {
  public static final int PORT = 10000;

  public static void main(String[] args) {
    SampleServer2 sm = new SampleServer2();
    try {
      ServerSocket ss = new ServerSocket(PORT);

      System.out.println("Waiting now ...");
      while(true) {
        try {
          Socket sc = ss.accept();
          System.out.println("Welcom!");

          ConnectToClient cc = new ConnectToClient (sc);
          cc.start();
        } catch (Exception ex) {
          ex.printStackTrace();
          break;
        }
      }
    } catch (Exception e) {
      e.printStackTrace();
    }
  }
}



class ConnectToClient extends Thread {

  private Socket sc;
  private BufferedReader br;
  private PrintWriter pw;

  public ConnectToClient(Socket s) {
    sc = s;
  }
  public void run() {
    try {
      br = new BufferedReader(new InputStreamReader(sc.getInputStream()));
      pw = new PrintWriter(new BufferedWriter(new OutputStreamWriter(sc.getOutputStream())));
    } catch (Exception e) {
      e.printStackTrace();
    }
    while (true) {
      try {
        String str = br.readLine();
        System.out.println(str);
        Random rnd = new Random();
        RandomStrings rs = new RandomStrings();
        pw.println("Server : [" + str.charAt(str.length()-1) + rs.GetRandomString(rnd.nextInt(10)) + "] (^_^)!");
        pw.flush();
      } catch (Exception e) {
        try {
          br.close();
          pw.close();
          sc.close();
          System.out.println("Good Bye !!");
          break;
        } catch (Exception ex) {
          ex.printStackTrace();
        }//TODO: handle exception
      }
    }
  }
}



class RandomStrings {

  private final String stringchar = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
  private Random rnd = new Random();
  private StringBuffer sbf = new StringBuffer(15);

  public String GetRandomString(int cnt) {
    for(int i=0; i<cnt; i++) {
      int val = rnd.nextInt(stringchar.length());
      sbf.append(stringchar.charAt(val));
    }
    return sbf.toString();
  }
}
```


### Client

```java
import java.awt.*;
import java.awt.event.*;
import java.io.*;
import java.net.*;
import javax.swing.*;

import javax.swing.JButton;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.JTextField;


public class Client extends JFrame implements Runnable{
  public static final String HOST = "localhost";

  public static final int PORT = 10000;

  private JTextField tf;
  private JTextArea ta;
  private JScrollPane sp;
  private JPanel pn;
  private JButton bt;

  private Socket sc;
  private BufferedReader br;
  private PrintWriter pw;

  public static void main(String[] args) {
    Client cl = new Client();
  }
  public Client() {
    super("Client Field");
    tf = new JTextField();
    ta = new JTextArea();
    sp = new JScrollPane(ta);
    pn = new JPanel();
    bt = new JButton("Send");

    pn.add(bt);
    add(tf, BorderLayout.NORTH);
    add(sp, BorderLayout.CENTER);
    add(pn, BorderLayout.SOUTH);

    bt.addActionListener(new SampleActionListener());
    addWindowListener(new SampleWindowListener());

    setSize(400, 300);
    setVisible(true);

    Thread th = new Thread(this);
    th.start();
  }

  public void run() {
    try {
      sc = new Socket(HOST, PORT);
      br = new BufferedReader(new InputStreamReader(sc.getInputStream()));
      pw = new PrintWriter(new BufferedWriter(new OutputStreamWriter(sc.getOutputStream())));

      while (true) {
        try {
          String str = br.readLine();
          ta.append(str + "\n");
        } catch (Exception e) {
          br.close();
          pw.close();
          sc.close();
          break;
        }
      }
    } catch (Exception ex) {
      ex.printStackTrace();
    }
  }
  public class SampleActionListener implements ActionListener {
    public void actionPerformed(ActionEvent e) {
      try {
        String str = tf.getText();
        pw.println(str);
        ta.append(str + "\n");
        pw.flush();
        tf.setText("");
      } catch (Exception ex) {
        ex.printStackTrace();
      }
    }
  }

  class SampleWindowListener extends WindowAdapter
  {
    public void windowClosing(WindowEvent e) {
      System.exit(0);
    }
  }
}

```

## compile & run

```shell
$ javac SampleServer2.java
$ javac Client.java

$ java SampleServer2
$ java Client
```

![](image-kowht894.png)

