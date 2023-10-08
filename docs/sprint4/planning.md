# RCOMP - SPRINT 4 - PLANNING

### Sprint Master: 

<hr>

## Sprint Backlog

<hr>

## Technical Information


### Language

We decided on Java as our preferred implementation language. 


### TCP Synchronization 

The message includes information about the number of bytes the data is made of. 


### Server application 

#### Server Port Number

Connection requests are received on port number 9999. 

#### Connection establishment

Client connects to the application with a Socket class, using server IP and the port number. 

```java
try { sock = new Socket(serverIP, 9999);}
        catch(IOException ex) {
            System.out.println("Failed to connect.");
            System.exit(1);
    }
```

If the connection is successful, server returns a new socket connected to the client. 

#### Sending and receiving data 

InputStream and OutputStream are used to send and receive data. 

The size of the data is calculated based on the 2 data length fields:

```java
int dLength1 = inputStream.read();
int dLength2 = inputStream.read();
int dataLength = dLength1 + 256 * dLength2;
```

Based on the code of the request message, a specific response with code 2 (ACK) or 3 (ERR - in case of failed authentication) is sent. 
If the code doesn't match any of the specified values, a response with the ERR code is sent:

```java
switch (code) {
        case 0:
            System.out.println("Received COMMTEST request");
            sendResponse(outputStream, 2, null);
            break;
        case 1:
            System.out.println("Received DISCONN request");
            sendResponse(outputStream, 2, null);
            try {
                remCli(clientSocket);
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
            System.out.println("Client: " + clientSocket.getInetAddress() + " disconnected");
            return; // Exit the method to close the connection
        case 4:
            System.out.println("Received AUTH request");
            boolean isAuthenticated = authenticateUser(data);
            if (isAuthenticated) {
                sendResponse(outputStream, 2, null);
            } else {
                sendResponse(outputStream, 3, "Authentication failed".getBytes());
            }
            break;
        default:
            System.out.println("Received unknown request with code: " + code);
            sendResponse(outputStream, 3, "Unknown request".getBytes());
            break;
}
```

Response handling:

```java
private static void sendResponse(OutputStream outputStream, int code, byte[] data) throws IOException {
        // Calculate the length of the data field
        int dataLength = (data != null) ? data.length : 0;
        int dLength1 = dataLength % 256;
        int dLength2 = dataLength / 256;

        // Build the SBP message
        byte[] message = new byte[4 + dataLength];
        message[0] = (byte) version;
        message[1] = (byte) code;
        message[2] = (byte) dLength1;
        message[3] = (byte) dLength2;
        if (dataLength > 0) {
            System.arraycopy(data, 0, message, 4, dataLength);
        }

        // Send the SBP message
        outputStream.write(message);
        outputStream.flush();
    }
```




#### Multi-threading

In order to handle multiple requests simultaneously, a thread is created for each socket. 

```java
while (true) {
        Socket clientSocket = sock.accept();
        System.out.println("Client connected: " + clientSocket.getInetAddress());
        // Handles client connection in a separate thread
        Thread clientThread = new Thread(() -> {
            try {processRequests(clientSocket);
            } catch (IOException e) {throw new RuntimeException(e);}});
        clientThread.start();
        addCli(clientSocket);
}
```
Adding client:

```java
 public static synchronized void addCli(Socket s) throws Exception {
        cliList.put(s,new DataOutputStream(s.getOutputStream()));
    }
```

When a request with code 1 (DISCONN) is received by the server, the client socket is removed:

```java
public static synchronized void remCli(Socket s) throws Exception {
    cliList.get(s).write(0);
    cliList.remove(s);
    s.close();
}
```

#### Authentication 

Authentication is achieved by sending an AUTH request from the client with a username and a password. 
Server responds eihter with code 2 (ACK) when the credentials are correct, or with code 3 (ERR) when authentication failed. 

```java
case 4:
    System.out.println("Received AUTH request");
    boolean isAuthenticated = authenticateUser(data);
    if (isAuthenticated) {
        sendResponse(outputStream, 2, null);
    } else {
        sendResponse(outputStream, 3, "Authentication failed".getBytes());
    }
    break;
```

```java
private static boolean authenticateUser(byte[] data) {
    String[] credentials = new String(data).split("\0");
    String username = credentials[0];
    String password = credentials[1];

    // Perform authentication logic here
    // Replace this with your actual authentication mechanism
    return (username.equals("username") && password.equals("password"));
}
```


### Client application 

#### Connecting to Server

In order to connect do the desired server the IPv4/IPv6 or DNS name should be given.
The port number is hardcoded in the application
If the server side application is running on the server and the above mentioned argument
is correct the connection will be established.

```java
        // Checks if ip argument given
        if(args.length!=1) {
            System.out.println(
                    "Server IPv4/IPv6 address or DNS name is required as argument");
            System.exit(1); }
        // Checks if host available
        try { serverIP = InetAddress.getByName(args[0]);
            System.out.println("Connected to server: "+serverIP);}
        catch(UnknownHostException ex) {
            System.out.println("Invalid server: " + args[0]);
            System.exit(1); }
        // Tries connection
        try { sock = new Socket(serverIP, 9999);}
        catch(IOException ex) {
            System.out.println("Failed to connect.");
            System.exit(1); }
```

#### Authentication

To authenticate the user has to give a valid username and password which is stored in the server.
In our case the user credentials are "username" and "password".
If the given credentials are correct then the user enters to the console of the Client Application.
Else the user can retry the login.

/Login interface/
```java
        // Tries authentication
        try{
            // Authenticate client
            System.out.print("Username: "); username = in.readLine();
            System.out.print("Password: "); password = in.readLine();
            boolean authenticated = authenticateUser(sock, username, password);
            while(!authenticated){
                System.out.println("Authentication failed. Please check your credentials.");
                System.out.print("Username: "); username = in.readLine();
                System.out.print("Password: "); password = in.readLine();
                authenticated = authenticateUser(sock, username, password);
            }
```

/Authentication process/
```java
private static boolean authenticateUser(Socket socket, String username, String password) throws IOException {
        // Concatenate username and password with null terminators
        byte[] userData = (username + '\0' + password + '\0').getBytes();

        // Send AUTH request with username and password
        sendRequest(socket, 4, userData);

        // Receive and process server response
        byte[] response = receiveResponse(socket);
        return (response.length == 1 && response[0] == 2); // Check if the response is ACK
    }
```



#### Sending requests

After establishing the connection and authenticating the user the client has a continous connection with the server.
The two request we can send from the client is COMMTEST and DISCONN. These request does not contain any data.
The data is always sent in the specified format. Version is hardcoded in the applciation. Code depends on the 
request (COMMTEST = 0;DISCONN = 1).

/Contious connection with server/
```java
    while(true) { // read messages from the console and send them to the server
    System.out.println("DISCONN to disconnect, COMMTEST to test connection");
            frase=in.readLine();
            //Disconnection request to server == END CONNECTION WITH CLIENT
            if(frase.compareTo("DISCONN")==0) {
                // Send DISCONN message
                sendRequest(sock, 1, null);
                // Receive and process server responses
                receiveResponse(sock);
                break;
            } else if (frase.compareTo("COMMTEST")==0) {
                // Send DISCONN message
                sendRequest(sock, 0, null);
                // Receive and process server responses
                receiveResponse(sock);}}
        // Close the TCP connection
        System.out.println("Bye bye...");
        sock.close();
```

/Coding the request/
```java
private static void sendRequest(Socket socket, int code, byte[] data) throws IOException {
        OutpuatStream outputStream = socket.getOutputStream();
        // Calculate the length of the data field
        int dataLength = (data != null) ? data.length : 0;
        int dLength1 = dataLength % 256;
        int dLength2 = dataLength / 256;

        // Build the SBP message
        byte[] message = new byte[4 + dataLength];
        message[0] = (byte) version;
        message[1] = (byte) code;
        message[2] = (byte) dLength1;
        message[3] = (byte) dLength2;
        if (dataLength > 0) {
            System.arraycopy(data, 0, message, 4, dataLength);
        }

        // Send the SBP message
        outputStream.write(message);
        outputStream.flush();
    }
```

#### Reciving response

If one request is succesful the server sends ACK response. If something went wrong the server sends ERR response.
While ACK contains no data ERR has data with error message.
The clien application reads the response and takes actions by the reply recived.

```java
private static byte[] receiveResponse(Socket socket) throws IOException {
        InputStream inputStream = socket.getInputStream();

        // Read the SBP message version
        int version = inputStream.read();

        // Read the SBP message code
        int code = inputStream.read();

        // Read the data length fields
        int dLength1 = inputStream.read();
        int dLength2 = inputStream.read();
        int dataLength = dLength1 + 256 * dLength2;

        // Read the data field
        byte[] data = new byte[dataLength];
        if (dataLength > 0) {
            int bytesRead = inputStream.read(data);
            if (bytesRead != dataLength) {
                throw new IOException("Failed to read complete data field");
            }
        }
        byte[] message = new byte[4+dataLength];
        message[0] = (byte) version;
        message[1] = (byte) code;
        message[2] = (byte) dLength1;
        message[3] = (byte) dLength2;
        if (dataLength > 0) {
            System.arraycopy(data, 0, message, 4, dataLength);
        }

        // Process the received response
        switch (code) {
            case 2:
                System.out.println("Received ACK message");
                break;
            case 3:
                System.out.println("Received ERR message: " + new String(data));
                break;
            default:
                System.out.println("Received unknown message with code: " + code);
                break;
        }
        return message;
    }
```