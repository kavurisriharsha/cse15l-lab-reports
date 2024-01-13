# **Lab Report - 1**

The folder structure of the workspace is as follows:

```bash
/home:
  lecture1

/home/lecture1:
  Hello.class Hello.java messages README

/home/lecture1/messages:
  en-us.txt es-mx.txt zh-cn.txt
```

The active user is `user`.

## `cd`

When `cd` is used with a folder as an argument, the working directory is changed to the specified folder.  
  <sub> Working directory is `/home/lecture1` and is being changed into `/home/lecture1/messages/`
  ```bash
  [user@sahara ~/lecture1]$ cd messages
  [user@sahara ~/lecture1/messages]$ pwd
  /home/lecture1/messages
  ```
  > This is intended behaviour and is the most common use case of `cd`, or "change directory".


When `cd` is used with a file as an argument, in this case, `en-us.txt`, an **error** is displayed as follows.  
  <sub> Working directory is `/home/lecture1/messages/`
  ```bash
  [user@sahara ~/lecture1/messages]$ cd en-us.txt 
  bash: cd: en-us.txt: Not a directory
  ```
  > This is displayed as `cd`, "change directory" is to be used with a directory as an argument. When it is used with a file, it returns an error as files cannot be switched into.


When `cd` is used without any arguments, the working directory is changed to `/home`.  
  <sub> Working directory is `/home/lecture1/messages/` and is being changed into `/home/`
  ```bash
  [user@sahara ~/lecture1/messages]$ cd
  [user@sahara ~]$ pwd
  /home
  ```
  > If this is used within the context of a different user, the working directory would be set to that user's home directory, i.e. `/home/[your username here]`.


## `ls`

When `ls` is used with a folder as an argument, the contents of the specified folder are displayed.  
  <sub> Working directory is `/home/`
  ```bash
  [user@sahara ~]$ ls lecture1/
  Hello.class  Hello.java  messages  README
  ```
  > `ls` stands for "list". So, it lists all the files and directories within the specified folder.


When `ls` is used with a file as an argument, the path of the file is displayed, as input in the argument(i.e. path is not resolved).  
  <sub> Working directory is `/home/`
  ```bash
  [user@sahara ~]$ ls lecture1/Hello.java
  lecture1/Hello.java
  [user@sahara ~]$ ls ./lecture1/Hello.java
  ./lecture1/Hello.java
  ```
  > This behavior is exhibited as `ls` cannot "list" the contents of a file. Since the file contains no files within itself, the file path, as given in the argument is displayed.

When `ls` is used without any arguments, the contents of the working directory are displayed.  
  <sub> Working directory is `/home/lecture1/`
  ```bash
  [user@sahara ~/lecture1]$ ls
  Hello.class  Hello.java  messages  README
  ```
  > The default argument that `ls` takes when no other argument is explicitly specified is `./` or the present directory. 


## `cat`

When `cat` is used with a folder as an argument, an **error** is displayed as follows.  
  <sub> Working directory is `/home/lecture1/`
  ```bash
  [user@sahara ~/lecture1]$ cat messages/
  cat: messages/: Is a directory
  ```
  > This message is displayed as `cat` is to be used with files and it doesn't know how directories are to be handled. To concatenate and display the contents of all the files within the `messages` directory, `cat messages/*` can be used instead.
 
When `cat` is used with a file as an argument, the contents of the file are displayed.  
  <sub> Working directory is `/home/lecture1/`
  ```bash
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
  > `cat` or "concatenate" takes one or more file-paths as arguments, which are all concatenated, or joined end to end and displayed in the order they were specified.

When `cat` is used with 2 files as arguments, the contents of the files are displayed in the order that they were specified.
  <sub> Working directory is `/home/lecture1`
  ```bash
  [user@sahara ~/lecture1]$ cat ./messages/en-us.txt ./messages/es-mx.txt ./messages/zh-cn.txt 
  Hello World!¡Hola Mundo!
  你好世界
  ```
> It is important to note that `cat` does not insert newline characters at the end of a file. The contents of the files are concatenated end to end as they occur in the file itself. In this particular example, the newline character has been removed from `en-us.txt` which results in the contents of `es-mx.txt` being printed immediately after i.e. starting on the same line without any whitespace immediately after the last character of `en-us.txt`. 

When `cat` is used with no arguments, it initially seems like it does nothing. However, the command is in fact waiting for the user input which it then prints in the next line. This can be exited using `Ctrl+C`.  
  <sub> Working directory is `/home/lecture1/`
  ```bash
  [user@sahara ~/lecture1]$ cat
  Input
  Input
  ^C
  ```
  > Since there is no file specified to be concatenated, `cat`, by default, displays the user input. The key combination `Ctrl+C` acts as an interrupt and breaks out of this loop.


