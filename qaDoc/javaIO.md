## 字节流

```java
byte b[] = new byte[1024];
FileInputStream fileInputStream = new FileInputStream("");
fileInputStream.read();
BufferedInputStream bufferedInputStream = new BufferedInputStream(fileInputStream);
bufferedInputStream.read(b);
int readNum;
while ((readNum = bufferedInputStream.read(b)) != -1) {
  FileOutputStream fileOutputStream = new FileOutputStream("");
  fileOutputStream.write(b, 0, readNum);
  fileOutputStream.flush();

  BufferedOutputStream bufferedOutputStream = new BufferedOutputStream(fileOutputStream);
  bufferedOutputStream.write(b, 0, readNum);
  bufferedOutputStream.flush();
}
```

## 字符流

```java
FileReader fileReader = new FileReader("");
BufferedReader bufferedReader = new BufferedReader(fileReader);
String line;
FileWriter fileWriter = new FileWriter("");
BufferedWriter bufferedWriter = new BufferedWriter(fileWriter);
while ((line = bufferedReader.readLine()) != null) {
  bufferedWriter.write(line);
  bufferedWriter.flush();
}
InputStreamReader gbkIn = new InputStreamReader(new FileInputStream(""), "GBK");
OutputStreamWriter gbkOut = new OutputStreamWriter(new FileOutputStream(""), "GBK");
```

## 对象流（克隆）

```java
ByteArrayOutputStream bout = new ByteArrayOutputStream();
ObjectOutputStream oos = new ObjectOutputStream(bout);
oos.writeObject(new Object());
ByteArrayInputStream bin = new ByteArrayInputStream(bout.toByteArray());
ObjectInputStream ois = new ObjectInputStream(bin);
ois.readObject();

ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(""));
ByteArrayInputStream bin = new ByteArrayInputStream(new FileInputStream(""));
```