# **Lab Report - 2**

The folder structure is as follows:
```bash
/home/gram:
  .ssh  wavelet

/home/gram/.ssh:
  id_rsa  id_rsa.pub  known_hosts

/home/gram/wavelet:
  ChatServer.class  README.md      Server.java   URLHandler.class
  ChatServer.java   Handler.class  Server.class  ServerHttpHandler.class

skavuri@ieng6.ucsd.edu:~/:
  .ssh

skavuri@ieng6.ucsd.edu:~/.ssh:
  authorized_keys
```

## ChatServer

The following code implements the expected functionality for the ChatServer program. 

```java
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {
    String chatMessages = "";

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return String.format("Chat Server\nUse /add-message?s=<string>&user=<string> to add message");
        }
        else {
            if (url.getPath().contains("/add-message")) {
                String[] parameters = url.getQuery().split("=");
                if (parameters[0].equals("s")) {
                    try {
                        chatMessages = chatMessages + String.format("%s: %s\n", parameters[2], parameters[1].split("&")[0]);
                        return chatMessages;
                    }
                    catch(Exception e){
                        return "404 Not Found!";
                    }
                }
            }
            return "404 Not Found!";
        }
    }
}

class ChatServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```

The code implements support for the following requests:
- `/`
- `/add-message?s=<string>&user=<string>`

When `/` is accessed, a message is displayed as follows that demonstrates the correct usage of the program.


`/add-message` expects 2 query parameters, namely - `s`, the message that the user wants to send, and `user`, the username of the current user. When either one of these parameters is defined illegally, a 404 error is thrown. 

> NOTE: The username can not be empty but the message string has no such rule.



