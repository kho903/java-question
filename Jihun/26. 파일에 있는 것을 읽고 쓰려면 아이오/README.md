# I/O 는...
- I/O는 우리가 만든 프로그램에 있는 어떤 내용을,
  - 파일에 읽거나 저장할 일이 있을 때
  - 다른 서버나 디바이스로 보낼 일이 있을 때
- 사용한다. I는 Input, O는 Output, 입출력 통칭 I/O
- I/O는 JVM을 기준으로 읽을 때에는 Input, 파일로 쓰거나 외부로 전송은 Output. JVM 기준이라는 것이 중요하다.
- 초기 단계 자바는 java.io 패키지의 클래스만 제공. 여기에는 바이트 기반의 데이터를 처리하기 위해서 여러 종류의 스트림(Stream)이라는 클래스를 제공한다. 읽는 작업은
InputStream을 통해서, 쓰는 작업은 OutputStream을 통해서 작업하도록 되어 있다.
- 바이트가 아닌 char 기반 문자열은 Reader, Writer로 처리. 읽을 때, Reader, 쓸 때, Writer.
- 스트림이라는 말은 번역하면 "개천, 줄기". 자바에서 스트림 (Stream)은 끊기지 않고 연속적인 데이터를 말한다.
- JDK 1.4부터 보다 빠른 I/O를 위한 NIO (New I/O)가 추가되었다. 스트림이 아닌 버퍼(Buffer)와 채널(Channel)기반으로 데이터를 처리.
- Java 7에서는 NIO2 가 추가됨. 파일을 보다 효율적으로 처리하기 위함. 기존 여러 단점 보완.

# 자바의 File 과 Files 클래스
- java.io 패키지의 File 클래스. 파일과 파일의 경로(path) 정보 포함. 정체가 불분명하고, 심볼릭 링크(symbolic link)와 같은 유닉스 계열의 파일에서 사용하는 몇몇
기능을 제대로 제공하지 못한다. 그래서 Java 7부터는 NIO2 등장해, java.nio.file 패키지의 Files 클래스에서 File 클래스에 있는 메소드들을 대체하여 젝송.
그리고, File 클래스는 객체를 생성하여 데이터를 처리하지만, Files 클래스는 모든 메소드가 static으로 별도 객체 생성 필요가 없다.
- File 클래스는 파일 및 경로 정보를 통제하기 위한 클래스다. File 클래스는 생성한 파일 객체가 가리키고 있는 것이
  - 존재하는지,
  - 파일인지 경로인지,
  - 읽거나, 쓰거나, 실행할 수 있는지
  - 언제 수정되었는지
- 를 확인하는 기능과 해당 파일의
  - 이름을 바꾸고,
  - 삭제하고,
  - 생성하고,
  - 전체 경로를 확인
- 하는 등의 기능을 제공한다. 이외에 File 객체가 가리키는 것이 파일이 아닌 경로일 경우에는 해당 경로에 있는
  - 파일의 목록을 가져오거나,
  - 경로를 생성하고,
  - 경로를 삭제하는
- 등의 기능도 있다. 생성자는 아래와 같다.

| 생성자                               | 설명                                                           |
|-----------------------------------|--------------------------------------------------------------|
| File(File parent, String child)   | 이미 생성되어 있는 File 객체(parent)와 그 경로의 하위 경로 이름으로 새로운 File 객체를 생성 |
| File(String pathname)             | 지정한 경로 이름으로 File 객체를 생성                                      |
| File(String parent, String child) | 상위 경로(parent)와 하위 경로(child)로 File 객체를 생성                     |
| File(URI uri)                     | URI에 따른 File 객체를 생성                                          |

- child는 경로, 파일 이름이 될 수 있다. 그래서 두 번째에 있는 pathname만 받는 생성자는 경로만 지정하는 것은 아니다. 만약 전체 경로와 파일이름이 pathname에
지정되어 있을 경우에는 파일을 가리키는 File 객체가 된다.
- URI는 Uniform Resource Identifier의 약자로, 어떠한 리소스를 가리키기 위한 경로를 뜻한다.

# File 클래스를 이용하여 파일의 경로, 상태 확인
```java
package vol2._26.io;

import static java.io.File.*;

import java.io.File;

public class FileSample {
	public static void main(String[] args) {
		FileSample sample = new FileSample();
		// String pathName = "/Users/kimjihun/dev/GodOfJavaEx/text";
		String pathName =
			separator + "Users" + separator + "kimjihun" + separator + "dev"
				+ separator + "GodOfJavaEx" + separator + "text";
		sample.checkPath(pathName);
	}

	private void checkPath(String pathName) {
		File file = new File(pathName);
		System.out.println(pathName + " is exists? = " + file.exists());
	}
}
```
```text
/Users/kimjihun/dev/GodOfJavaEx/text is exists? = true
```
- pathName의 문자열에서 OS마다 각 디렉터리를 구분하는 기호가 달라, 모호함을 없애기 위해 File 클래스의 separator라는 것이 static 변수로 존재한다. 해당 변수를
사용하여 문자열을 나타내는 것이 안전하다.
- 해당 매개 변수를 사용하여 File 객체를 생성하고, exists() 메소드로 해당 경로가 존재하는 지 확인하고 있다. 존재하면 true 아니면 false 리턴
- 존재하지 않는 디렉터리를 File 클래스를 사용하여 만들려면 mkdir()나 mkdirs() 메소드를 사용하면 된다. mkdir()은 디렉터리 하나, mkdirs()는 여러 개의 
하위 디렉터리를 만든다.
```java
public void makeDir(String pathName) {
    File file = new File(pathName);
    System.out.println("Make " + pathName + " result = " + file.mkdir());
}

public void makeDirs(String pathName) {
    File file = new File(pathName);
    System.out.println("Make " + pathName + " result = " + file.mkdirs());
}
```
```text
Make /Users/kimjihun/dev/GodOfJavaEx/text/newDir result = true
Make /Users/kimjihun/dev/GodOfJavaEx/text/newDirs/newOne result = true
```
- 위와 같이 하나면 mkdir(), 여러 개면 mkdirs()를 사용할 수 있다.
- 그리고, 해당 File 객체가 파일인지, 경로인지, 숨김파일인지 확인하려면 아래와 같이 사용할 수 있다.
```java
public void checkFileMethods(String pathName) {
    File file = new File(pathName);
    System.out.println(pathName + " is Directory? = " + file.isDirectory());
    System.out.println(pathName + " is file? = " + file.isFile());
    System.out.println(pathName + " is hidden? = " + file.isHidden());
}
```
```text
/Users/kimjihun/dev/GodOfJavaEx/text is Directory? = true
/Users/kimjihun/dev/GodOfJavaEx/text is file? = false
/Users/kimjihun/dev/GodOfJavaEx/text is hidden? = false
/Users/kimjihun/dev/GodOfJavaEx/text/newDir is Directory? = true
/Users/kimjihun/dev/GodOfJavaEx/text/newDir is file? = false
/Users/kimjihun/dev/GodOfJavaEx/text/newDir is hidden? = false
```
- File 클래스의 메소드를 수행하고 있는 자바 프로그램이 해당 File 객체에 읽거나, 쓰거나, 실행할 수 있는 권한이 있는지를 확인할 수도 있다.
```java
public void canFileMethods(String pathName) {
    File file = new File(pathName);
    System.out.println(pathName + " can Read? = " + file.canRead());
    System.out.println(pathName + " can Write? = " + file.canWrite());
    System.out.println(pathName + " can Execute? = " + file.canExecute());
}
```
```text
/Users/kimjihun/dev/GodOfJavaEx/text can Read? = true
/Users/kimjihun/dev/GodOfJavaEx/text can Write? = true
/Users/kimjihun/dev/GodOfJavaEx/text can Execute? = true
/Users/kimjihun/dev/GodOfJavaEx/text/newDir can Read? = true
/Users/kimjihun/dev/GodOfJavaEx/text/newDir can Write? = true
/Users/kimjihun/dev/GodOfJavaEx/text/newDir can Execute? = true
```
- canExecute() 메소드는 Java 6 부터 추가.
- 파일이나 경로가 언제 생성되었는지를 확인하는 File 클래스의 메소드는 lastModified()다. long 타입을 반환하여, java,util 패키지의 Date 클래스로 시간 확인 가능
```java
public void lastModified(String pathName) {
    File file = new File(pathName);
    System.out.println(pathName + " last modified = " + new Date(file.lastModified()));
}
```
```text
/Users/kimjihun/dev/GodOfJavaEx/text last modified = Sun Oct 23 15:58:14 KST 2022
/Users/kimjihun/dev/GodOfJavaEx/text/newDir last modified = Sun Oct 23 15:58:14 KST 2022
```
- 마지막으로 파일을 삭제하고자 할 때 delete 메소드를 사용하면 된다. 이 메소드는 정상적으로 삭제한 경우에 true를 리턴한다.
- 하위에 다른 디렉토리가 있을 경우 false 리턴.
```text
public void deleteMethod(String pathName) {
    File file = new File(pathName);
    System.out.println(pathName + " delete() = " + file.delete());
}
```
```text
/Users/kimjihun/dev/GodOfJavaEx/text/newDir delete() = true
/Users/kimjihun/dev/GodOfJavaEx/text/newDirs/newOne delete() = true
/Users/kimjihun/dev/GodOfJavaEx/text/newDirs delete() = true
```

# File 클래스를 이용하여 파일을 처리하자.
- createNewFile() 메소드는 비어 있는 새로운 파일을 생성한다.
```java
package vol2._26.io;

import static java.io.File.separator;

import java.io.File;
import java.io.IOException;

public class FileManageClass {
	public static void main(String[] args) {
		FileManageClass sample = new FileManageClass();
		String pathName =
			separator + "Users" + separator + "kimjihun" + separator + "dev"
				+ separator + "GodOfJavaEx" + separator + "text";
		String fileName = "test.txt";

		sample.checkFile(pathName, fileName);
	}

	private void checkFile(String pathName, String fileName) {
		File file = new File(pathName, fileName);
		try {
			System.out.println("Create result = " + file.createNewFile());
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}
```
```text
Create result = true
```
- 해당 경로에 test.txt이 생성되어 있을 것이다. 아무 데이터를 추가하지 않아, 데이터는 없을 것이다. 그리고, 다시 이 프로그램을 실행하면, 실행결과는 false일 것이다.
- 다음은 파일의 정보를 확인하는 메소드이다.
```java
private void checkFile(String pathName, String fileName) {
    File file = new File(pathName, fileName);
    try {
        System.out.println("Create result = " + file.createNewFile());
        getFileInfo(file);
    } catch (IOException e) {
        e.printStackTrace();
    }
}

private void getFileInfo(File file) throws IOException {
    System.out.println("Absolute path = " + file.getAbsolutePath());
    System.out.println("Absolute file = " + file.getAbsoluteFile());
    System.out.println("Canonical path = " + file.getCanonicalPath());
    System.out.println("Canonical file = " + file.getCanonicalFile());

    System.out.println("Name = " + file.getName());
    System.out.println("Path = " + file.getPath());
}
```
```text
Create result = false
Absolute path = /Users/kimjihun/dev/GodOfJavaEx/text/test.txt
Absolute file = /Users/kimjihun/dev/GodOfJavaEx/text/test.txt
Canonical path = /Users/kimjihun/dev/GodOfJavaEx/text/test.txt
Canonical file = /Users/kimjihun/dev/GodOfJavaEx/text/test.txt
Name = test.txt
Path = /Users/kimjihun/dev/GodOfJavaEx/text/test.txt
```
- Absolute와 Canonical 결과가 동일한데, Absolute는 "절대" 경로를 의미. Canonical은 "절대적이고, 유일한" 경로를 의미. file의 객체의 경로가 상대 경로일
경우에 결과가 달라진다.
- 파일이름을 제외한 경로만을 확인하려면 다음과 같이 getParent() 메소드를 사용할 수 있다.
```java
System.out.println("Parent = " + file.getParent());
```
```text
Parent = /Users/kimjihun/dev/GodOfJavaEx/text
```

# 디렉터리에 있는 목록을 살펴보기 위한 list 메소드들
| 리턴 타입         | 메소드 이름 및 매개 변수                   | 설명                                                                                    |
|---------------|----------------------------------|---------------------------------------------------------------------------------------|
| static File[] | listRoots()                      | JVM이 수행되는 OS에서 사용중인 파일 시스템의 루트(root) 디렉터리 목록을 File 배열로 리턴. static 메소드로 별도 객체 생성 필요 없음 |
| String[]      | list()                           | 현재 디렉터리의 하위에 있는 목록을 String 배열로 리턴                                                     |
| String[]      | list(FilenameFilter filter)      | 현재 디렉터리의 하위에 있는 목록 중, 매개 변수로 넘어온 filter의 조건에 맞는 목록을 String 배열로 리턴                     |
| File[]        | listFiles()                      | 현재 디렉터리의 하위에 있는 목록을 File 배열로 리턴                                                       |
| File[]        | listFiles(FileFilter filter)     | 현재 디렉터리의 하위에 있는 목록 중, 매개 변수로 넘어온 filter의 조건에 맞는 목록을 File 배열로 리턴                       |
| File[]        | listFiles(FilenameFilter filter) | 현재 디렉터리의 하위에 있는 목록 중, 매개 변수로 넘어온 filter의 조건에 맞는 목록을 File 배열로 리턴                       |

- listRoots()는 말 그대로, 루트 디렉터리 목록 제공. 예를 들어 윈도우의 경우 "C:\", "D:\"와 같이 각 드라이브의 루트 디렉터리 목록 제공. 유닉스 계열 OS는 "/"와 함께
추가로 마운트되어 있는 파일 시스템의 루트 디렉터리 목록이 제공됨
- File 클래스에서 제공하는 파일 목록을 확인하는 메소드는 list(), listFiles()이다. list()는 String 배열 리턴, listFiles()는 File 배열을 리턴한다.
매개 변수로 넘어가는 FileFilter와 FilenameFilter는 목록을 가져올 때, 필요한 파일만 선택할 수 있는 필터이다.
- 먼저 FileFilter 인터페이스에 선언되어 있는 메소드는 다음과 같다.

| 리턴 타입   | 메소드 이름 및 매개 변수        | 설명                             |
|---------|-----------------------|--------------------------------|
| boolean | accept(File pathname) | 매개 변수로 넘어온 File 객체가 조건에 맞는지 확인 |

- FilenameFilter 인터페이스에 선언된 메소드는 다음과 같다.

| 리턴 타입   | 메소드 이름 및 매개 변수                | 설명                                                   |
|---------|-------------------------------|------------------------------------------------------|
| boolean | accept(File dir, String name) | 매개 변수로 넘어온 디렉토리(dir)에 있는 경로나 파일 이름(name)이 조건에 맞는지 확인 |

- 두 개 모두 인터페이스로 선언되어 있다. 따라서, 어떤 조건을 주려면, 이 인터페이스를 구현해야만 한다. 구현한 클래스의 객체를 list로 시작하는 메소드의 매개 변수로
넘겨주면, 메소드에서 파일이나 경로를 만날 때마다 여기 있는 accept() 메소드가 자동으로 수행된다. 따라서, accept() 메소드를 어떻게 구현했느냐에 따라서, 필요한
파일의 목록이 달라진다. 

```java
package vol2._26.io;

import java.io.File;
import java.io.FileFilter;

public class JPGFileFilter implements FileFilter {
	@Override
	public boolean accept(File pathname) {
		if (pathname.isFile()) {
			String fileName = pathname.getName();
			if (fileName.endsWith(".jpg")) return true;
		}
		return false;
	}
}
```
- JPGFileFilter에서는 파일 객체가 디렉터리가 아닌 파일인지 확인한다. 파일 이름이 ".jpg"로 끝나면 true를 리턴하고, 그 외의 모든 경우에는 false를 리턴
```java
package vol2._26.io;

import java.io.File;
import java.io.FilenameFilter;

public class JPGFilenameFilter implements FilenameFilter {
	@Override
	public boolean accept(File dir, String name) {
		if (name.endsWith(".jpg")) return true;
		return false;
	}
}
```
- FilenameFilter는 메소드 매개 변수로 name이 넘어오기 때문에 별도로 File 객체의 getName() 메소드를 호출할 필요가 없다. 
- FilenameFilter가 더 좋아보이지만, 디렉터리와 파일을 구분하지 못하기 때문에 만약 .jpg로 끝나는 디렉터리가 있으면 필터로 걸러낼 수 없다.
- 먼저 .jpg 파일과 .jpg로 끝나는 디렉터리를 만들자.

```shell
$ text > ll
total 0
drwxr-xr-x  2 kimjihun  staff    64B Oct 23 17:13 test.jpg
-rw-r--r--  1 kimjihun  staff     0B Oct 23 16:26 test.txt
-rw-r--r--  1 kimjihun  staff     0B Oct 23 17:13 겨울.jpg
-rw-r--r--  1 kimjihun  staff     0B Oct 23 17:13 석양.jpg
-rw-r--r--  1 kimjihun  staff     0B Oct 23 17:13 수련.jpg
-rw-r--r--  1 kimjihun  staff     0B Oct 23 17:13 푸른 언덕.jpg
```
- test.jpg는 디렉터리 이다.

```java
package vol2._26.io;

import static java.io.File.*;

import java.io.File;

public class FileFilterSample {
	public static void main(String[] args) {
		FileFilterSample sample = new FileFilterSample();
		String pathName =
			separator + "Users" + separator + "kimjihun" + separator + "dev"
				+ separator + "GodOfJavaEx" + separator + "text";
		sample.checkList(pathName);
	}

	private void checkList(String pathName) {
		File file;
		try {
			file = new File(pathName);
			File[] mainFileList = file.listFiles();
			// File[] mainFileList = file.listFiles(new JPGFilenameFilter());
			// File[] mainFileList = file.listFiles(new JPGFileFilter());
			for (File tempFile : mainFileList) {
				System.out.println(tempFile.getName());
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```
```text
석양.jpg
푸른 언덕.jpg
수련.jpg
test.jpg
test.txt
겨울.jpg
```
- 디렉토리를 포함한 모든 파일이 출력된다. 주석처리된 JPGFileNameFilter 를 사용한 결과는 어떻게 될까?
- JPGFilenameFilter에 있는 accept() 메소드는 매개 변수로 String 만을 받기 때문에, 해당 파일이 디렉터리인지 파일인지 알지 못한다. 따라서, .jpg로 끝나는지만
확인이 가능하므로, 결과는 다음과 같다.
```text
석양.jpg
푸른 언덕.jpg
수련.jpg
test.jpg
겨울.jpg
```
- test.jpg는 디렉터리이지만, 같이 출력이 된다. 그럼 다음 주석처리된 JPGFileFilter를 사용하면 어떻게 될까?
```text
석양.jpg
푸른 언덕.jpg
수련.jpg
겨울.jpg
```
- JPGFileFilter의 accept() 메소드의 경우 디렉터리를 제외한 file만 걸러주는 것을 확인할 수 있다.

# InputStream과 OutputStream은 자바 스트림의 부모들
- 자바의 I/O는 기본적으로 InputStream과 OutputStream 이라는 abstract 클래스를 통해서 제공된다. 따라서 어떤 대상의 데이터를 읽을 때에는 InputStream의 자식
클래스를 통해서 읽으면 되며, 어떤 대상의 데이터를 쓸 때에는 OutputStream의 자식 클래스를 통해서 쓰면 된다.
- 먼저 InputStream의 선언문이다.
```java
public abstract class InputStream
extends Object
implements Closeable
```
- abstract 클래스이므로, 이 클래스를 확장한 자식 클래스들을 살펴보아야 한다. 여기서 구현한 Closeable 인터페이스는 close() 메소드 하나만 선언되어 있는데,
어떤 리소스를 열었던 간에 이 잍너페이스를 구현하면 해당 리소스는 close() 를 이용하여 닫으라는 것을 의미한다. 이름에서 알 수 있듯이, "닫을 수 있다."는 것을 의미한다.
하지만, 해당 리소스를 다른 클래스에서도 작업할 수 잇도록, java.io 패키지에 있는 클래스를 사용할 때에는 작업 종료시 close()로 "항상" 닫아 주어야 한다.
- "리소스"라는 것은 파일이 될 수도 있고, 네트워크 연결도 될 수 있다. 스트림을 통해서 작업할 수 있는 모든 것을 리소스라고 생각하면 된다.
- InputStream 클래스에 선언된 메소드는 다음과 같다.

| 리턴 타입        | 메소드 이름 및 매개 변수                   | 설명                                                                                             |
|--------------|----------------------------------|------------------------------------------------------------------------------------------------|
| int          | available()                      | 스트림에서 중단없이 읽을 수 있는 바이트의 개수를 리턴                                                                 |
| void         | mark(int readlimit)              | 스트림의 현재 위치를 표시(mark)해 둔다. 여기서 매개 변수로 넘긴 int 값은 표시해둔 자리의 최대 유효 길이다. 넘어가면 표시해둔 자리는 더이상 의미가 없어진다. |
| void         | reset()                          | 현재 위치를 mark() 메소드가 호출되었던 위치로 되돌린다.                                                             |
| boolean      | markSupported()                  | mark()나 reset() 메소드가 수행 가능한지를 확인                                                               |
| abstract int | read()                           | 스트림에서 다음 바이트를 읽는다. 이 클래스에 선언된 유일한 abstract 메소드                                                 |
| int          | read(byte[] b)                   | 매개 변수로 넘어온 바이트 배열에 데이터를 담는다. 리턴값은 데이터를 담은 개수                                                   |
| int          | read(byte[] b, int off, int len) | 매개 변수로 넘어온 바이트 배열에 특정 위치(off)부터 지정한 길이(len)만큼의 데이터를 담는다. 리턴값은 데이터를 담은 개수                       |
| long         | skip(long n)                     | 매개 변수로 넘어온 길이(n) 만큼의 데이터를 건너 뛴다.                                                               |
| void         | close()                          | 스트림에서 작업중인 대상을 해제한다. 이 메소드를 수행한 이후에는 다른 메소드를 사용하여 데이터를 처리할 수 없다.                               |

- read()와 close()는 중요하다. 데이터를 읽을 때에는 read() 메소드를 사용하면 되고, 해당 리소스를 닫을 때에는 close() 메소드를 호출해야 한다. 스트림을 다룰 때,
다른 메소드를 호출하지 않아도 close()는 반드시 호출해야 한다. 그렇지 않으면 심각한 오류 발생.
- InputStream을 확장한 주요 클래스들은 다음과 같다.
```text
AudioInputStream, ByteArrayInputStream, FileInputStream, FilterInputStream, InputStream, ObjectInputStream, PipedInputStream, 
SequenceInputStream, StringBufferInputStream
```
- 많이 사용하는 스트림은 다음 3개이다.

| 클래스               | 설명                                                                                  |
|-------------------|-------------------------------------------------------------------------------------|
| FileInputStream   | 파일을 읽는데 사용한다. 주로 쉽게 읽을 수 있는 텍스트 파일을 읽기 위한 용도라기 보다, 이미지와 같이 바이트코드로 된 데이터를 읽을 때 사용한다. |
| FilterInputStream | 이 클래스는 다른 입력 스트림을 포괄하며, 단순히 InputStream 클래스가 Override 되어 있다.                        |
| ObjectInputStream | ObjectOuputStream으로 저장한 데이터를 읽는 데 사용한다.                                             |

- FileInputStream과 ObjectInputStream은 객체를 생성해서 데이터를 처리하면 된다. 하지만, FileInputStream의 생성자는 protected로 선언되어 있기 때문에,
상속 받은 클래스에서만 객체를 생성할 수 있다. 
- FileInputStream을 확장한 클래스는 다음과 같다.
```text
BufferedInputStream, CheckedInputStream, CipherInputStream, DataInputStream, DeflaterInputStream, DigestInputStream, 
InflaterInputStream, LineNumberInputStream, ProgressMonitorInputStream, PushbackInputStream, 
```
- OutputStream의 선언문은 다음과 같다.
```text
public abstract class OuputStream
extends Object
implements Closeable, Flushable
```
- InputStream과 마찬가지로 abstract 클래스로 선언되어 있다. 그리고, Closeable과 Flushable 두 개의 인터페이스를 구현하였다.
- Flushable 인터페이스도 flush()라는 메소드 하나만 선언되어 있으며, 일반적으로 어떤 리소스에 데이터를 쓸 때, 매번 쓰기 작업을 "요청할 때마다 저장"하면 효율이
안좋아진다. 따라서 대부분 저장을 할 떄 버퍼(buffer)를 갖고 데이터를 차곡차곡 쌓아 두었다가, 어느 정도 차게 되면 한번에 쓰는 것이 좋다. 그러한 버퍼를 사용할 때,
flush() 메소드는 "현재 버퍼에 있는 내용을 기다리지 말고 무조건 저장해" 라고 시키는 것이다.
- OutputStream 메소드는 다음과 같다.

| 리턴 타입 | 메소드 이름 및 매개 변수                    | 설명                                                  |
|-------|-----------------------------------|-----------------------------------------------------|
| void  | write(byte[] b)                   | 매개 변수로 받은 바이트 배열(b)를 저장                             |
| void  | write(byte[] b, int off, int len) | 매개 변수로 받은 바이트 배열(b)의 특정 위치(off)부터 지정한 길이(len)만큼 저장  |
| void  | write(int b)                      | 매개 변수로 받은 바이트를 저장한다. 타입은 int지만, 실제 저장되는 것은 바이트로 저장됨 |
| void  | flush()                           | 버퍼에 쓰려고 대기하고 있는 데이터를 강제로 쓰도록 한다.                    |
| void  | close()                           | 쓰기 위해 열은 스트림을 해제한다.                                 |
- InputStream과 마찬가지로 close() 메소드로 닫아 주어야 한다.

# Reader와 Writer
- 지금까지 살펴본 Stream은 byte를 다루기 위한 것이며, Reader와 Writer는 char 기반의 문자열을 처리 하기 위한 클래스다. 즉, 우리가 일반적인 텍스트 에디터로
쉽게 볼 수 있는 파일들을 처리하기 위한 클래스라고 보면 된다.
- Reader의 선언부는 다음과 같다.
```text
public abstract class Reader
extends Object
implements Readable, Closeable
```
- Reader 역시 abstract 클래스로, abstract으로 선언된 메소드는 close()와 read() 이다. 그리고, InputStream과 많이 중복된다.

| 리턴 타입         | 메소드 이름 및 매개 변수                      | 설명                                                                                          |
|---------------|-------------------------------------|---------------------------------------------------------------------------------------------|
| boolean       | ready()                             | Reader에서 작업할 대상이 읽을 준비가 되어 있는지를 확인                                                          |
| void          | mark(int readAheadLimit)            | Reader의 현재 위치를 표시(mark)해 둔다. readAheadLimit는 최대 유효 길이로, 이 값이 넘어가면, 표시해 둔 자리는 더 이상 의미가 없어진다. |
| void          | reset()                             | 현재 위치를 mark() 메소드가 호출되었던 위치로 되돌린다.                                                          |
| boolean       | markSupported()                     | mark()나 reset() 메소드가 수행 가능한지를 확인한다.                                                         |
| int           | read()                              | 하나의 char를 읽는다.                                                                              |
| int           | read(char[] cbuf)                   | 매개 변수로 넘어온 char 배열의 데이터를 담는다. 리턴값은 데이터를 담은 개수다.                                             |
| abstract int  | read(char[] cbuf, int off, int len) | 매개 변수로 넘어온 char 배열에 특정 위치(off) 부터 지정한 길이(len)만큼의 데이터를 담는다. 리턴값은 데이터를 담은 개수다.                |
| int           | read(CharBuffer target)             | 매개 변수로 넘어온 CharBuffer 클래스의 객체에 데이터를 담는다. 리턴 값은 데이터를 담은 개수다.                                 |
| long          | skip(long n)                        | 매개 변수로 넘어온 개수 만큼의 char를 건너뛴다.                                                               |
| abstract void | close()                             | Reader 에서 작업중인 대상을 해제한다. 이 메소드를 수행한 이후에는 다른 메소드를 사용하여 데이터를 처리할 수 없다.                        |

- Reader를 확장한 주요 클래스들은 다음과 같다.
```text
BufferedReader, CharArrayReader, FilterReader, InputStreamReader, PipedReader, StringReader
```
- 여기서 BufferedReader, InputStreamReader가 제일 많이 사용된다.
- Writer 클래스의 선언부는 다음과 같다.
```text
public abstract class Writer
extends Object
implements Appendable, Closeable, Flushable
```
- 마찬가지로 abstract 클래스이다. Appendable 인터페이스는 Java 5 부터 추가되었으며, 각종 문자열을 추가하기 위해 선언되었다.
- Writer 클래스도 OutputStream 클래스에 선언된 메소드와 대부분 동일하지만, append() 라는 메소드가 존재한다는 점이 다르다.

| 리턴 타입         | 메소드 이름 및 매개 변수                               | 설명                                                                            |
|---------------|----------------------------------------------|-------------------------------------------------------------------------------|
| Writer        | append(char c)                               | 매개 변수로 넘어온 char를 추가                                                           |
| Writer        | append(charSequence csq)                     | 매개 변수로 넘어온 CharSequence를 추가                                                   |
| Writer        | append(charSequence csq, int start, int end) | 매개 변수로 넘어온 CharSequence를 추가하며, 쓰여지는 해당 문자열의 시작 위치(start)와 끝 위치(end)를 지정하면 된다. |
| void          | write(char[] cbuf)                           | 매개 변수로 넘어온 char의 배열을 추가                                                       |
| abstract void | write(char[] cbuf, int off, int len)         | 매개 변수로 넘어온 char의 배열의 특정 위치(off)부터 특정 길이(len)만큼을 추가                            |
| void          | write(int c)                                 | 매개 변수로 넘어온 int 값에 해당하는 char를 추가                                               |
| void          | write(String str)                            | 매개 변수로 넘어온 문자열을 쓴다.                                                           |
| void          | write(String str, int off, int len)          | 매개 변수로 넘어온 문자열을 추가하며, 쓰여지는 해당 문자열의 시작 위치(start)와 끝 위치(end)를 지정하면 된다.          |
| abstract void | flush()                                      | 버퍼에 있는 데이터를 강제로 대상 리소스에 쓰도록 한다.                                               |
| abstract void | close()                                      | 쓰기 위해 열은 스트림을 해제한다.                                                           |

- 이 중에서 append() 메소드의 매개 변수로 넘어가는 CharSequence는 인터페이스로, String, StringBuffer, StringBuilder가 대표적인 구현 클래스이다.
그래서 CharSequence를 넘긴다는 것은 대부분의 문자열을 다 받아서 처리한다는 말이다.
- Writer 클래스는 JDK 1.1 부터 제공되었는데, 그 때 데이터를 저장하기 위한 메소드는 write() 밖에 없었다. 만약 만들어진 문자열이 String 타입이라면 write()를
사용해도 별 상관은 없겠지만, StringBuilder 나 StringBuffer로 문자열을 만들면 append() 메소드를 사용하는 것이 훨씬 편할 것이다.

# 텍스트 파일을 써보자
- char 기반의 내용을 파일로 쓰기 위해서는 FileWriter라는 클래스를 사용한다. 이 클래스의 생성자는 다음과 같다.

| 생성자                                         | 설명                                                                              |
|---------------------------------------------|---------------------------------------------------------------------------------|
| FileWriter(File file)                       | File 객체를 매개 변수로 받아 객체를 생성                                                       |
| FileWriter(File file, boolean append))      | File 객체를 매개 변수로 받아 객체를 생성. append 값을 통해 해당 파일의 뒤에 붙일지 (true), 덮어쓸지(false)를 정한다. |
| FileWriter(FileDescriptor fd)               | FileDescriptor 객체를 매개 변수로 받아 객체를 생성                                             |
| FileWriter(String fileName)                 | 지정한 문자열의 경로와 파일 이름에 해당하는 객체를 생성                                                 |
| FileWriter(String fileName, boolean append) | 지정한 문자열의 경로와 파일 이름에 해당하는 객체를 생성. append 값에 따라서, 데이터를 추가할지 덮어쓸지 결정.              |

- 이 Writer에 있는 write()나 append() 메소드를 사용하여 데이터를 쓰면, 메소드를 호출했을 때마다 파일에 쓰기 때문에 매우 비효율적이다. 이러한 단점을 보완하기 위해서
BufferedWriter라는 클래스가 있다.

| 생성자                                  | 설명                                                  |
|--------------------------------------|-----------------------------------------------------|
| BufferedWriter(Writer out)           | Writer 객체를 매개 변수로 받아 객체를 생성                         |
| BufferedWriter(Writer out, int size) | Writer 객체를 매개 변수로 받아 객체를 생성. size를 이용해 버퍼의 크기를 정한다. |

- 이름 그대로, BufferedWriter는 버퍼라는 공간에 저장할 데이터를 보관해 두었다가, 버퍼가 차게 되면 데이터를 저장하도록 도와준다. 따라서, 매우 효율적인 저장이 가능
매개 변수로 Writer를 받듯이, FileWriter를 사용하면 파일에 저장할 수 있다.
- 0부터 10까지의 값을 한 줄에 하나씩 텍스트 파일에 저장하는 예제는 다음과 같다.
```java
package vol2._26.io;

import static java.io.File.*;

import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class ManageTextFile {
	public static void main(String[] args) {
		ManageTextFile manager = new ManageTextFile();
		int numberCount = 10;
		String fullPath =
			separator + "Users" + separator + "kimjihun" + separator + "dev"
				+ separator + "GodOfJavaEx" + separator + "text" + separator + "numbers.text";
		manager.writeFile(fullPath, numberCount);
	}

	private void writeFile(String fileName, int numberCount) {
		FileWriter fileWriter = null;
		BufferedWriter bufferedWriter = null;
		try {
			fileWriter = new FileWriter(fileName);
			bufferedWriter = new BufferedWriter(fileWriter);
			for (int i = 0; i <= numberCount; i++) {
				bufferedWriter.write(Integer.toString(i));
				bufferedWriter.newLine();
			}
			System.out.println("Write Success !!");
		} catch (IOException e) {
			e.printStackTrace();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			if (bufferedWriter != null) {
				try {
					bufferedWriter.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
			if (fileWriter != null) {
				try {
					fileWriter.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
	}
}
```
- 먼저 `fileWriter = new FileWriter(fileName);`은 파일에 쓰는 작업을 하기 위해 FileWriter 객체를 생성한다. 이 객체를 생성할 때, IOException 발생
가능성으로 try-catch문. 객체의 사용이 끝나면 close()로 닫아준다.
- `bufferedWriter = new BufferedWriter(fileWriter);`에서 파일에 쓰기 작업을 할 때 버퍼를 사용하기 위해 BufferedWriter 객체 생성.
  객체의 사용이 끝나면 close()로 닫아준다.
- `bufferedWriter.newLine()` 은 줄바꿈을 해준다.
```text
FileWriter 객체를 생성할 때, IOException이 발생할 수 있다. 이 예외는 일반적으로 다음 상황에서 발생.
- 매개 변수로 넘어온 파일이름이 파일이 아닌 경로를 의미할 경우
- 해당 파일이 존재하지는 않지만, 권한등의 문제로 생성할 수가 없는 경우
- 파일이 존재하지만, 여러 가지 이유로 파일을 열 수가 없는 경우
```
- 여기서 주의할 점은 try문 내에서 FileWriter, BufferedWriter를 선언했다면 finally에서 close() 메소드를 호출할 수 없다.
- finally() 에서 close() 를 하는 이유는 try 끝에서 close()를 구현했다면, 중간에 예외가 발생했을 때 close() 메소드가 호출되지 않는다. 이를 피하려면
catch 블록에서 일일이 close()를 처리해야 하는데, 이러한 단점을 해결하기 위해 finally 블록에서 close() 처리를 한다.
- 또 한가지 규칙은 FileWriter, BufferedWriter 순으로 객체를 생성했으므로, 객체를 닫는 순서는 BufferedWriter, FileWriter 순으로 닫아야만 한다.
즉, 가장 마지막에 연(open) 객체부터 닫아주어야 정상적인 처리가 가능.
```text
Write Success !!
```
```text
0
1
2
3
4
5
6
7
8
9
10
```
- 정상적으로 수행되었다면 위와 같은 `Write Success !!`문구와 numbers.txt 내에 위와 0 ~ 10 까지의 숫자가 줄바꿈과 함께 포함되어 있어야 한다.
- 만약 `fileWriter = new FileWriter(filename, true);`와 같이 true 라는 매개 변수를 두 번째로 넘겨주면, 기존 파일 끝에 새로운 내용이 추가된다.
true를 false로 바꾸고 실행하면, 저장된 값ㄷ이 없어지고 0 ~ 10까지의 숫자가 한 번만 저장되어 있을 것이다.
- 다시 말해서 true는 파일 붙여쓰기, false는 덮어쓰기가 된다. (default는 false)

# 텍스트 파일을 읽어보자
- 파일을 바로 다시 여는 방법은 FileReader, BufferedReader를 사용하면 된다.
```java
public void readFile(String fileName) {
    FileReader fileReader = null;
    BufferedReader bufferedReader = null;
    try {
        fileReader = new FileReader(fileName);
        bufferedReader = new BufferedReader(fileReader);
        String data;
        while ((data = bufferedReader.readLine()) != null) {
            System.out.println(data);
        }
        System.out.println("Read success !!");
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        if (bufferedReader != null) {
            try {
                bufferedReader.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        if (fileReader != null) {
            try {
                fileReader.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```
- 쓰기 부분과 큰 차이가 없다. 다른 부분은 다음과 같다.
```java
String data;
while ((data = bufferedReader.readLine()) != null) {
    System.out.println(data);
}
```
- data라는 String 객체를 만들었다. 그리고 while문에서 bufferedReader 객체의 readLine() 메소드를 호출하여 데이터를 읽어들였다. 그리고 바로 data라는 변수에
그 값을 할당했다. 여기서 data와 bufferedRead.readLine() 사이에 = 으로 readLine() 결과를 data에 담는 작업을 수행한다. 그 이후에
data가 null인지 확인한다. 풀어서 쓰면 다음과 같다.
```text
data = bufferedReader.readLine();
data != null;
```
- while의 소괄호 내에 두 개 이상의 문장이 포함될 수 없기 때문에 이와 같은 방법을 일반적으로 많이 사용한다.
- 수행 결과는 다음과 같다.
```text
0
1
2
3
4
5
6
7
8
9
10
Read success !!
```
- 이렇게 코드를 작성하면 코드의 길이도 길어지고, 가독성도 많이 떨어진다. 따라서 java.util 패키지에 있는 Scanner를 사용하면 쉽게 파일을 읽을 수 있다.
```java
public void readFileWithScanner(String fileName) {
    File file = new File(fileName);
    Scanner scanner = null;
    try {
        scanner = new Scanner(file);
        while (scanner.hasNextLine()) {
            System.out.println(scanner.nextLine());
        }
        System.out.println("Read success!!");
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        if (scanner != null) {
            scanner.close();
        }
    }
}
```
```text
0
1
2
3
4
5
6
7
8
9
10
Read success!!
```
- Scanner 클래스는 텍스트 기반의 기본 자료형이나 문자열 데이터를 처리하기 위한 클래스다. 정규 표현식도 사용하여 데이터를 잘라 처리 가능. Scanner 클래스는 
생성자의 종류가 다양한데, 여기서는 File의 객체를 매개 변수로 받아 파일의 내용을 읽는 데에 사용했다. hasNextLine() 이라는 메소드는 다음 줄이 있는지 확인하기 위해
사용되며, nextLine() 메소드는 다음 줄의 내용을 문자열로 한 줄씩 리턴해 준다. 따라서, 이 예제에서는 while 문의 조건식에는 hasNextLine() 메소드로 다음 줄이
있는지를 확인한 후, nextLine() 메소드로 한 줄씩 읽어들이도록 하였다.
- 추가로 Java 7에서 제공하는 Files라는 클래스를 사용하면 다음과 같이 한 줄로 파일을 읽을 수도 있다.
```java
String data = new String(Files.readAllBytes(Paths.get(fileName)));
```

# 정리
- Java 7 이상을 사용한다면 java.nio.file 패키지의 Files 클래스를 더 많이 사용할 것이다.
- InputStream/OutputStream 과 Reader/Writer 의 차이점도 알고 있어야 한다.
