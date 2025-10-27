---
title: "Java Socket Programming - Lập trình mạng với TCP/UDP"
date: 2024-02-20T14:00:00+07:00
draft: false
description: "Hướng dẫn lập trình mạng trong Java: Socket, ServerSocket, TCP Client-Server, UDP Communication"
tags: ["Java", "Socket", "Networking", "TCP", "UDP"]
categories: ["Java", "Lập trình mạng"]
author: "Trung Tín"
showAuthor: true
showDate: true
showDateUpdated: true
showReadingTime: true
showTableOfContents: true
showPagination: true
showTaxonomies: true
showWordCount: true
---

{{< lead >}}
🌐 Build **networked applications** với Java Socket Programming - từ chat apps, file transfer đến multiplayer games!
{{< /lead >}}

## Giới thiệu về Socket Programming

{{< alert "circle-info" >}}
**💡 Network Power:** Socket là foundation của **mọi ứng dụng mạng** - web servers, chat apps, online games đều dùng Socket!
{{< /alert >}}

**Socket Programming** là kỹ thuật lập trình cho phép các chương trình giao tiếp qua mạng. Socket là endpoint của communication channel giữa 2 máy tính.

### Các khái niệm cơ bản

- **IP Address**: Địa chỉ định danh máy tính (VD: 192.168.1.1, localhost)
- **Port**: Số cổng (0-65535) định danh ứng dụng
- **Protocol**: Giao thức (TCP, UDP)
- **Client**: Khởi tạo connection
- **Server**: Lắng nghe và chấp nhận connection

## TCP vs UDP

| TCP (Transmission Control Protocol) | UDP (User Datagram Protocol) |
|--------------------------------------|------------------------------|
| Connection-oriented | Connectionless |
| Reliable (đảm bảo dữ liệu) | Unreliable (có thể mất data) |
| Ordered delivery | No ordering |
| Slower | Faster |
| Used: HTTP, FTP, Email | Used: Video streaming, Gaming, DNS |

## TCP Socket Programming

### 1. Simple TCP Server

```java
import java.io.*;
import java.net.*;

public class SimpleTCPServer {
    public static void main(String[] args) {
        int port = 8080;
        
        try (ServerSocket serverSocket = new ServerSocket(port)) {
            System.out.println("🚀 Server started on port " + port);
            System.out.println("⏳ Waiting for client...");
            
            // Chấp nhận client connection
            Socket clientSocket = serverSocket.accept();
            System.out.println("✅ Client connected: " + 
                clientSocket.getInetAddress().getHostAddress());
            
            // Input stream - nhận data từ client
            BufferedReader in = new BufferedReader(
                new InputStreamReader(clientSocket.getInputStream())
            );
            
            // Output stream - gửi data tới client
            PrintWriter out = new PrintWriter(
                clientSocket.getOutputStream(), true
            );
            
            // Đọc message từ client
            String clientMessage = in.readLine();
            System.out.println("📩 Client says: " + clientMessage);
            
            // Gửi response tới client
            out.println("Server received: " + clientMessage);
            
            // Đóng connection
            clientSocket.close();
            System.out.println("🔌 Connection closed");
            
        } catch (IOException e) {
            System.err.println("❌ Server error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### 2. Simple TCP Client

```java
import java.io.*;
import java.net.*;

public class SimpleTCPClient {
    public static void main(String[] args) {
        String hostname = "localhost";
        int port = 8080;
        
        try (Socket socket = new Socket(hostname, port)) {
            System.out.println("🔌 Connected to server");
            
            // Output stream - gửi data tới server
            PrintWriter out = new PrintWriter(
                socket.getOutputStream(), true
            );
            
            // Input stream - nhận data từ server
            BufferedReader in = new BufferedReader(
                new InputStreamReader(socket.getInputStream())
            );
            
            // Gửi message tới server
            String message = "Hello from Client!";
            out.println(message);
            System.out.println("📤 Sent to server: " + message);
            
            // Đọc response từ server
            String serverResponse = in.readLine();
            System.out.println("📥 Server response: " + serverResponse);
            
        } catch (UnknownHostException e) {
            System.err.println("❌ Unknown host: " + hostname);
        } catch (IOException e) {
            System.err.println("❌ I/O error: " + e.getMessage());
        }
    }
}
```

## Multi-Client TCP Server

```java
import java.io.*;
import java.net.*;
import java.util.concurrent.*;

public class MultiClientTCPServer {
    private static final int PORT = 8080;
    private static final ExecutorService threadPool = 
        Executors.newFixedThreadPool(10);
    
    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            System.out.println("🚀 Multi-Client Server started on port " + PORT);
            
            while (true) {
                // Chấp nhận client connection
                Socket clientSocket = serverSocket.accept();
                System.out.println("✅ New client connected: " + 
                    clientSocket.getInetAddress());
                
                // Xử lý client trong thread riêng
                threadPool.execute(new ClientHandler(clientSocket));
            }
            
        } catch (IOException e) {
            System.err.println("❌ Server error: " + e.getMessage());
        } finally {
            threadPool.shutdown();
        }
    }
    
    // Inner class xử lý mỗi client
    private static class ClientHandler implements Runnable {
        private Socket socket;
        
        public ClientHandler(Socket socket) {
            this.socket = socket;
        }
        
        @Override
        public void run() {
            try (
                BufferedReader in = new BufferedReader(
                    new InputStreamReader(socket.getInputStream())
                );
                PrintWriter out = new PrintWriter(
                    socket.getOutputStream(), true
                )
            ) {
                String clientAddress = socket.getInetAddress().toString();
                System.out.println("🔧 Handling client: " + clientAddress);
                
                // Đọc messages từ client
                String message;
                while ((message = in.readLine()) != null) {
                    System.out.println("📩 [" + clientAddress + "]: " + message);
                    
                    // Echo back
                    out.println("Echo: " + message);
                    
                    // Exit command
                    if (message.equalsIgnoreCase("exit")) {
                        break;
                    }
                }
                
                System.out.println("🔌 Client disconnected: " + clientAddress);
                
            } catch (IOException e) {
                System.err.println("❌ Error handling client: " + e.getMessage());
            } finally {
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## Interactive TCP Client

```java
import java.io.*;
import java.net.*;
import java.util.Scanner;

public class InteractiveTCPClient {
    public static void main(String[] args) {
        String hostname = "localhost";
        int port = 8080;
        
        try (
            Socket socket = new Socket(hostname, port);
            PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
            BufferedReader in = new BufferedReader(
                new InputStreamReader(socket.getInputStream())
            );
            Scanner scanner = new Scanner(System.in)
        ) {
            System.out.println("🔌 Connected to server");
            System.out.println("Type messages (type 'exit' to quit):");
            
            while (true) {
                // Input từ user
                System.out.print("\n> ");
                String message = scanner.nextLine();
                
                // Gửi tới server
                out.println(message);
                
                // Exit
                if (message.equalsIgnoreCase("exit")) {
                    System.out.println("👋 Goodbye!");
                    break;
                }
                
                // Nhận response
                String response = in.readLine();
                System.out.println("📥 Server: " + response);
            }
            
        } catch (IOException e) {
            System.err.println("❌ Error: " + e.getMessage());
        }
    }
}
```

## Chat Application

### Chat Server

```java
import java.io.*;
import java.net.*;
import java.util.*;
import java.util.concurrent.*;

public class ChatServer {
    private static final int PORT = 8080;
    private static Set<ClientHandler> clients = 
        ConcurrentHashMap.newKeySet();
    
    public static void main(String[] args) {
        System.out.println("💬 Chat Server started on port " + PORT);
        
        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            while (true) {
                Socket socket = serverSocket.accept();
                ClientHandler client = new ClientHandler(socket);
                clients.add(client);
                new Thread(client).start();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    // Broadcast message tới tất cả clients
    public static void broadcast(String message, ClientHandler sender) {
        for (ClientHandler client : clients) {
            if (client != sender) {
                client.sendMessage(message);
            }
        }
    }
    
    public static void removeClient(ClientHandler client) {
        clients.remove(client);
    }
    
    private static class ClientHandler implements Runnable {
        private Socket socket;
        private PrintWriter out;
        private String username;
        
        public ClientHandler(Socket socket) {
            this.socket = socket;
        }
        
        @Override
        public void run() {
            try (
                BufferedReader in = new BufferedReader(
                    new InputStreamReader(socket.getInputStream())
                )
            ) {
                out = new PrintWriter(socket.getOutputStream(), true);
                
                // Nhận username
                out.println("Enter your username:");
                username = in.readLine();
                System.out.println("✅ " + username + " joined the chat");
                
                // Thông báo join
                broadcast(username + " joined the chat!", this);
                
                // Nhận messages
                String message;
                while ((message = in.readLine()) != null) {
                    if (message.equalsIgnoreCase("/exit")) {
                        break;
                    }
                    
                    System.out.println("[" + username + "]: " + message);
                    broadcast(username + ": " + message, this);
                }
                
            } catch (IOException e) {
                System.err.println("Error: " + e.getMessage());
            } finally {
                // Cleanup
                if (username != null) {
                    System.out.println("❌ " + username + " left the chat");
                    broadcast(username + " left the chat.", this);
                }
                
                removeClient(this);
                
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        
        public void sendMessage(String message) {
            out.println(message);
        }
    }
}
```

### Chat Client

```java
import java.io.*;
import java.net.*;
import java.util.Scanner;

public class ChatClient {
    private static final String HOSTNAME = "localhost";
    private static final int PORT = 8080;
    
    public static void main(String[] args) {
        try (
            Socket socket = new Socket(HOSTNAME, PORT);
            PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
            BufferedReader in = new BufferedReader(
                new InputStreamReader(socket.getInputStream())
            );
            Scanner scanner = new Scanner(System.in)
        ) {
            System.out.println("💬 Connected to chat server");
            
            // Thread đọc messages từ server
            Thread readerThread = new Thread(() -> {
                try {
                    String message;
                    while ((message = in.readLine()) != null) {
                        System.out.println("\n" + message);
                        System.out.print("> ");
                    }
                } catch (IOException e) {
                    System.err.println("Connection lost");
                }
            });
            readerThread.start();
            
            // Main thread gửi messages
            String message;
            while (scanner.hasNextLine()) {
                message = scanner.nextLine();
                out.println(message);
                
                if (message.equalsIgnoreCase("/exit")) {
                    break;
                }
            }
            
        } catch (IOException e) {
            System.err.println("Error: " + e.getMessage());
        }
    }
}
```

## UDP Socket Programming

### UDP Server

```java
import java.net.*;

public class UDPServer {
    private static final int PORT = 9876;
    private static final int BUFFER_SIZE = 1024;
    
    public static void main(String[] args) {
        try (DatagramSocket socket = new DatagramSocket(PORT)) {
            System.out.println("🚀 UDP Server started on port " + PORT);
            
            byte[] receiveBuffer = new byte[BUFFER_SIZE];
            
            while (true) {
                // Nhận packet
                DatagramPacket receivePacket = 
                    new DatagramPacket(receiveBuffer, receiveBuffer.length);
                socket.receive(receivePacket);
                
                // Xử lý data
                String message = new String(
                    receivePacket.getData(), 
                    0, 
                    receivePacket.getLength()
                );
                
                InetAddress clientAddress = receivePacket.getAddress();
                int clientPort = receivePacket.getPort();
                
                System.out.println("📩 Received from " + clientAddress + 
                    ":" + clientPort + " - " + message);
                
                // Gửi response
                String response = "Echo: " + message;
                byte[] sendBuffer = response.getBytes();
                
                DatagramPacket sendPacket = new DatagramPacket(
                    sendBuffer,
                    sendBuffer.length,
                    clientAddress,
                    clientPort
                );
                
                socket.send(sendPacket);
            }
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### UDP Client

```java
import java.net.*;
import java.util.Scanner;

public class UDPClient {
    private static final String HOSTNAME = "localhost";
    private static final int PORT = 9876;
    private static final int BUFFER_SIZE = 1024;
    
    public static void main(String[] args) {
        try (
            DatagramSocket socket = new DatagramSocket();
            Scanner scanner = new Scanner(System.in)
        ) {
            InetAddress serverAddress = InetAddress.getByName(HOSTNAME);
            System.out.println("🔌 Connected to UDP server");
            System.out.println("Type messages (type 'exit' to quit):");
            
            while (true) {
                // Input
                System.out.print("\n> ");
                String message = scanner.nextLine();
                
                if (message.equalsIgnoreCase("exit")) {
                    break;
                }
                
                // Gửi packet
                byte[] sendBuffer = message.getBytes();
                DatagramPacket sendPacket = new DatagramPacket(
                    sendBuffer,
                    sendBuffer.length,
                    serverAddress,
                    PORT
                );
                socket.send(sendPacket);
                
                // Nhận response
                byte[] receiveBuffer = new byte[BUFFER_SIZE];
                DatagramPacket receivePacket = 
                    new DatagramPacket(receiveBuffer, receiveBuffer.length);
                socket.receive(receivePacket);
                
                String response = new String(
                    receivePacket.getData(),
                    0,
                    receivePacket.getLength()
                );
                
                System.out.println("📥 Server: " + response);
            }
            
            System.out.println("👋 Goodbye!");
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## File Transfer over Socket

```java
// Server - nhận file
public class FileReceiver {
    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(8080)) {
            System.out.println("Waiting for file...");
            
            Socket socket = serverSocket.accept();
            System.out.println("Client connected");
            
            // Nhận file
            BufferedInputStream in = new BufferedInputStream(
                socket.getInputStream()
            );
            
            FileOutputStream fileOut = new FileOutputStream("received_file.txt");
            BufferedOutputStream out = new BufferedOutputStream(fileOut);
            
            byte[] buffer = new byte[4096];
            int bytesRead;
            
            while ((bytesRead = in.read(buffer)) != -1) {
                out.write(buffer, 0, bytesRead);
            }
            
            out.flush();
            out.close();
            
            System.out.println("✅ File received successfully!");
            
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// Client - gửi file
public class FileSender {
    public static void main(String[] args) {
        try (Socket socket = new Socket("localhost", 8080)) {
            System.out.println("Sending file...");
            
            // Đọc file
            FileInputStream fileIn = new FileInputStream("file_to_send.txt");
            BufferedInputStream in = new BufferedInputStream(fileIn);
            
            // Gửi qua socket
            BufferedOutputStream out = new BufferedOutputStream(
                socket.getOutputStream()
            );
            
            byte[] buffer = new byte[4096];
            int bytesRead;
            
            while ((bytesRead = in.read(buffer)) != -1) {
                out.write(buffer, 0, bytesRead);
            }
            
            out.flush();
            in.close();
            
            System.out.println("✅ File sent successfully!");
            
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## Best Practices

1. **Always close resources**: Sử dụng try-with-resources
2. **Handle exceptions**: IOException, SocketException
3. **Use multithreading**: Xử lý nhiều clients
4. **Buffer I/O**: BufferedReader/Writer cho performance
5. **Set timeouts**: socket.setSoTimeout() tránh block vô hạn
6. **Validate input**: Kiểm tra data từ network
7. **Use thread pools**: ExecutorService cho scalability

## Kết luận

{{< timeline >}}

{{< timelineItem icon="network-wired" header="TCP Socket" badge="Reliable" >}}
{{< badge >}}Socket{{< /badge >}} {{< badge >}}ServerSocket{{< /badge >}} - Connection-oriented, perfect cho chat, file transfer!
{{< /timelineItem >}}

{{< timelineItem icon="paper-plane" header="UDP Socket" badge="Fast" >}}
{{< badge >}}DatagramSocket{{< /badge >}} {{< badge >}}DatagramPacket{{< /badge >}} - Connectionless, ideal cho streaming, gaming!
{{< /timelineItem >}}

{{< timelineItem icon="users" header="Real Applications" badge="Production" >}}
Multi-client server, chat apps, file transfer - build anything với Socket Programming!
{{< /timelineItem >}}

{{< /timeline >}}

{{< alert "check" >}}
**✅ Best Practice:**
- Luôn dùng **try-with-resources** để auto-close sockets
- Handle **IOException** properly
- Use **BufferedReader/Writer** cho performance
- Implement **multithreading** cho multi-client servers
{{< /alert >}}

## Tài liệu tham khảo

{{< button href="https://docs.oracle.com/javase/tutorial/networking/" target="_blank" >}}
📚 Oracle Networking Tutorial
{{< /button >}}

{{< button href="https://docs.oracle.com/javase/8/docs/api/java/net/Socket.html" target="_blank" >}}
📖 Socket API Documentation
{{< /button >}}

{{< button href="https://www.pearson.com/en-us/subject-catalog/p/computer-networking/P200000003334" target="_blank" >}}
📕 Computer Networking Book
{{< /button >}}

