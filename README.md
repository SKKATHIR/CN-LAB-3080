EX 2
import java.io.*;
import java.net.URL;

public class Download {
    public static void main(String[] args) {
        try {
            String fileName = "digital_image_processing.jpg";
            String website = "http://tutorialspoint.com/java_dip/images/" + fileName;

            System.out.println("Downloading File From: " + website);

            URL url = new URL(website);
            InputStream inputStream = url.openStream();
            OutputStream outputStream = new FileOutputStream(fileName);

            byte[] buffer = new byte[2048];
            int length;

            while ((length = inputStream.read(buffer)) != -1) {
                outputStream.write(buffer, 0, length);
            }

            inputStream.close();
            outputStream.close();

            System.out.println("Download Completed");

        } catch (Exception e) {
            System.out.println("Exception: " + e.getMessage());
        }
    }
}
EX 3 A SERVER PROGRAM
import java.net.*;
import java.io.*;

public class Server {
    public static final int PORT = 4000;

    public static void main(String args[]) {
        try (ServerSocket sersock = new ServerSocket(PORT)) {
            System.out.println("Server Started");

            try (Socket sock = sersock.accept()) {
                System.out.println("Client Connected");

                BufferedReader ins = new BufferedReader(
                        new InputStreamReader(sock.getInputStream()));
                System.out.println("Client says: " + ins.readLine());

                PrintWriter ios = new PrintWriter(
                        sock.getOutputStream(), true);
                ios.println("Hello from server");
            }

        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
CLIENT PROGRAM  
import java.io.*;
import java.net.*;

public class Client {
    public static void main(String args[]) {
        try {
            System.out.println("Trying to connect...");
            Socket sock = new Socket(InetAddress.getLocalHost(), 4000);

            PrintWriter ps = new PrintWriter(sock.getOutputStream(), true);
            ps.println("Hi from client");

            BufferedReader is = new BufferedReader(
                    new InputStreamReader(sock.getInputStream()));
            System.out.println("Server says: " + is.readLine());

            sock.close();

        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
EX 3 B SERVER PROGRAM
import java.io.*;
import java.net.*;

class tcpchatserver {
    public static void main(String args[]) {
        try {
            ServerSocket Srv = new ServerSocket(4000);
            System.out.println("Server started... waiting for client");

            Socket Clt = Srv.accept();
            System.out.println("Client connected");

            PrintWriter toClient = new PrintWriter(
                    new BufferedWriter(
                            new OutputStreamWriter(Clt.getOutputStream())), true);

            BufferedReader fromClient = new BufferedReader(
                    new InputStreamReader(Clt.getInputStream()));

            BufferedReader fromUser = new BufferedReader(
                    new InputStreamReader(System.in));

            String CltMsg, SrvMsg;

            while (true) {
                CltMsg = fromClient.readLine();
                if (CltMsg == null || CltMsg.equals("end"))
                    break;

                System.out.println("Client says: " + CltMsg);
                System.out.print("Message to Client: ");
                SrvMsg = fromUser.readLine();
                toClient.println(SrvMsg);
            }

            Clt.close();
            Srv.close();

        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
CLIENT PROGRAM
import java.io.*;
import java.net.*;

class tcpchatclient {
    public static void main(String args[]) {
        try {
            Socket Clt = new Socket(InetAddress.getLocalHost(), 4000);

            PrintWriter toServer = new PrintWriter(
                    new BufferedWriter(
                            new OutputStreamWriter(Clt.getOutputStream())), true);

            BufferedReader fromServer = new BufferedReader(
                    new InputStreamReader(Clt.getInputStream()));

            BufferedReader fromUser = new BufferedReader(
                    new InputStreamReader(System.in));

            System.out.println("Connected to Server. Type 'end' to Quit.");

            String CltMsg, SrvMsg;

            while (true) {
                System.out.print("Message to Server: ");
                CltMsg = fromUser.readLine();
                toServer.println(CltMsg);

                if (CltMsg.equals("end"))
                    break;

                SrvMsg = fromServer.readLine();
                System.out.println("Server says: " + SrvMsg);
            }

            Clt.close();

        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}

