# **Lab Report - 1**


## `cd`

When `cd` is used with a folder as an argument, the working directory is changed to the specified folder.
```
[user@sahara ~/lecture1]$ cd messages
[user@sahara ~/lecture1/messages]$ pwd
/home/lecture1/messages
```

When `cd` is used with a file as an argument, in this case, `en-us.txt`, an **error** is displayed as follows.
```
[user@sahara ~/lecture1/messages]$ cd en-us.txt 
bash: cd: en-us.txt: Not a directory
```

When `cd` is used without any arguments, the working directory is changed to `/home`.
```
[user@sahara ~/lecture1/messages]$ cd
[user@sahara ~]$ pwd
/home
```
If this is used within the context of a different user, the working directory would be set to that user's home directory.


## `ls`

When `ls` is used with a folder as an argument, the contents of the specified folder are displayed.
```
[user@sahara ~]$ ls lecture1/
Hello.class  Hello.java  messages  README
```

When `ls` is used with a file as an argument, the path of the file is displayed, as input in the argument(i.e. path is not resolved).
```
[user@sahara ~]$ ls lecture1/Hello.java
lecture1/Hello.java
[user@sahara ~]$ ls ./lecture1/Hello.java
./lecture1/Hello.java
```

When `ls` is used without any arguments, the contents of the working directory are displayed.
```
[user@sahara ~/lecture1]$ ls
Hello.class  Hello.java  messages  README
```


## `cat`

When `cat` is used with a folder as an argument, an **error** is displayed as follows.
```
[user@sahara ~/lecture1]$ cat messages/
cat: messages/: Is a directory
```

When `cat` is used with a file as an argument, the contents of the file are displayed.
```
[user@sahara ~/lecture1]$ cat Hello.java 
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;

public class Hello {
  public static void main(String[] args) throws IOException {
    String content = Files.readString(Path.of(args[0]), StandardCharsets.UTF_8);    
    System.out.println(content);
  }
}
```

When `cat` is used with no arguments, it initially seems like it does nothing. However, the command is in fact waiting for the user input which it then prints in the next line. This can be exited using `Ctrl+C`.
```
[user@sahara ~/lecture1]$ cat
Input
Input
^C
```


