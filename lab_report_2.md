# Lab Report 2 - VSCode and Your Local Machine (Week 3)
***

## Part 1
Code for ChatServer (Built off of NumberServer of Week 2):
```
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class Handler implements URLHandler {
    // The one bit of state on the server: a number that will be manipulated by
    // various requests.
    int num = 0;
    ArrayList<String> users = new ArrayList<>();
    ArrayList<String> messages = new ArrayList<>();

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            String chatStr = "";
            for(int i = 0; i < users.size(); i++){
                chatStr += users.get(i) + ": " + messages.get(i) + '\n';
            }
            return chatStr;
        } else {
            if (url.getPath().contains("/add-message")) {
                String[] parameters = url.getQuery().split("&");
                String[] user = parameters[1].split("=");
                String[] message = parameters[0].split("=");
                if (message[0].equals("s")&&user[0].equals("user")) {
                    messages.add(message[1]);
                    users.add(user[1]);
                    String chatStr = "";
                    for(int i = 0; i < users.size(); i++){
                        chatStr += users.get(i) + ": " + messages.get(i) + '\n';
                    }
                    return chatStr;
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

***

![Image](lab-report-2a.png)
* handleRequest() will be called.
* The url will be passed into the method as an argument. Before the code runs, both the messages and users ArrayList feilds are empty.
* The handleRequest() method will update the messages and users feilds by adding in the strings after "s=" and "user=" in the url, respectively.

![Image](lab-report-2b.png)
* handleRequest() will be called again.
* The url will be passed into the method as an argument. Before the code runs, the messages and users ArrayList feilds will each have one string, "Hello" and "jpolitz", respectively.
* The handleRequest() method will update the messages and users feilds by adding in the strings after "s=" and "user=" in the url, respectively.

## Part 2
* ![Image](lab-report-2c.png)
* ![Image](lab-report-2d.png)
* ![Image](lab-report-2e.png)


## Part 3
From lab 2, I learned how to create and manipulate a web server. More specifically, I learned how to read input from the url to manipulate the data and how to output that data to the web server.
