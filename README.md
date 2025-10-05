# 95-702 Distributed Systems For Information Systems Management
## Project 3 Blockchain and Signatures       Spring 2025


Assigned: Monday, February 24, 2025
Due Monday, March 17, 2025 at 2:00 pm

### Important Note

Place your email address and full name at top of each source code file.

### Principles

One of our primary objectives in this course is to make clear the fundamental
distinction between functional and nonfunctional characteristics of distributed
systems. The functional characteristics describe the business or organizational
purpose of the system. The non-functional characteristics affect the quality of
the system. Is it fast? Does it easily interoperate with others? Is it fault
tolerant? Is it reliable and secure?

In this project, we will illustrate an important nonfunctional characteristic
of blockchain technology - its tamper evident design. We will build a stand-alone
blockchain in Task 0 and a distributed system where a remote client interacts with
a blockchain API in Task 1. In Task 2, we will work with digital signatures.


### Prerequisites

This prerequisite section discusses Gson. This same material appeared in the Appendix of Project 2. If you have already worked through that appendix, you may safely skip this prerequisite section.

In this project we will be using the Gson class to parse JSON messages. In this prerequisite section, there is guidance on setting up Gson in Intellij. JSON is a popular data format. It competes with XML. Either JSON or XML is appropriate to transfer textual data from one machine to another. Be sure to review the JSON grammar at www.json.org.

0. Create a new project named TestGsonProject and select "Maven" as the build system.
1. Edit the pom.xml file and include this XML element at the end of the file (before the closing project tag).
```
<dependencies>
    <dependency>
        <groupId>com.google.code.gson</groupId>
        <artifactId>gson</artifactId>
        <version>2.9.0</version>
    </dependency>
</dependencies>

```
2. Select File/Project Structure/Libraries/+/From Maven/.
   Using the search bar, search for com.google.code.gson.
   And then select com.google.code.gson:2.9.0.

3. Include the following import in your source code:

```
import com.google.gson.Gson;
```    

4. Test by compiling and running the following program (you may need to add a package):

```
import com.google.gson.Gson;

public class Main {
    public static void main(String[] args) {
        System.out.println("Hello world!");
        Gson gson = new Gson();
        System.out.println("Gson is available!");
    }
}
```

5. Suppose we have a message object that we want to serialize to JSON. Why do this?
We may want to transmit the data over a network in an interoperable and textual way.

```
package org.example;
import com.google.gson.Gson;

class Message {
    String name;
    int id;
    public Message(String name, int id) {
        this.name = name;
        this.id = id;
    }
}

public class Main {
    public static void main(String[] args) {
        // Create a message
        Message msg = new Message("Alice", 30);
        // Create a Gson object
        Gson gson = new Gson();
        // Serialize to JSON
        String messageToSend = gson.toJson(msg);
        // Display the JSON string
        System.out.println(messageToSend);
    }
}
```

6. Suppose we receive a message as a JSON string. We may want to deserialize
the JSON string to a Java object. Why do this? This is a huge convenience.
We do not have to parse the message ourselves.

```
package org.example;
import com.google.gson.Gson;

class Message {
    String name;
    int id;
    public Message(String name, int id) {
        this.name = name;
        this.id = id;
    }
}

public class Main {
    public static void main(String[] args) {
        // Create a message
        Message msg = new Message("Alice", 30);
        // Create a Gson object
        Gson gson = new Gson();
        // Serialize to JSON
        String messageToSend = gson.toJson(msg);
        // Display the JSON string
        System.out.println(messageToSend);

        // Suppose we receive the following JSON string from a network or file.
        // Double quotes would be used in a real message. Single quotes are used
        // here because we are doing this within a Java program.
        String someJSON = "{'id':45,'name':'Bob'}";
        Message incommingMsg = gson.fromJson(someJSON,Message.class);
        System.out.println(incommingMsg.name);
        System.out.println(incommingMsg.id);

    }
}
```
7. As a warm up exercise, write a program that reads a financial transaction from the keyboard. The transaction
will be expressed as a JSON string. Use Gson to build a Java object from the JSON string. Display the values
within the object and then use Gson to display the Java object in JSON format.

8. It is fine to use a large language model for this exercise. It is also fine to simply code this yourself.

Here is an example execution:
```
Enter a transaction in Json format. Include from, to, and amount.       
{"from":"Mike","to":"Marty","amount":123.50}      This line is entered by the user.
From: Mike                                        Display values within the object
To: Marty
Amount: 123.5
{"from":"Mike","to":"Marty","amount":123.5}       Use Gson to generate the JSON

```

### Project 3 Overview

In Task 0, you will write a blockchain by carefully following the directions found in our JavaDoc:

[See Block JavaDoc](https://www.andrew.cmu.edu/course/95-702/examples/javadoc/blockchaintask0/Block.html).

[See BlockChain Javadoc](https://www.andrew.cmu.edu/course/95-702/examples/javadoc/blockchaintask0/BlockChain.html).

Note that the Javadoc describes writing "data" or a "transaction" to the blockchain. In this project, our "data" or "transaction" will be simple statements transferring "DSCoin" from one player to another.

The Javadoc describes two classes that you need to write - Block.java and BlockChain.java.

In Task 1, you will distribute the application that you created in Task 0. You will write a client server application. The interaction between the client and the server will be with JSON messages (using Gson) over TCP sockets. This is similar to what you did in Project 2 but here we are using TCP rather than UDP.

You will be submitting complete Java programs and console screen interactions on a single PDF file. These should be clearly labelled as described below.

You will also be submitting three IntelliJ projects as three zip files.
See the very end of this document for a precise description of what needs to be submitted.

Documenting code is important. Be sure to provide comments in your code explaining what the code is doing and why.

Be sure to separate concerns when appropriate. It is fine to add methods that you feel are appropriate.

Be sure to include comments within your methods.

Data validation (of user input) is very important but we are not doing that here.

Any code from external sources, e.g., stack overflow or a large language model (LLM), **must be clearly cited with a URL or, in the case of an LLM, a comment saying what tool it is that you are using**.

Note that if you do use code from an external source or an LLM, you are still responsible for that code. That is, you may be asked questions about the code on exams. You will not have access to stack overflow or an LLM on exams.


## Task 0 A Standalone Blockchain Simulator

Write a solution to Task 0 by studying the Javadoc provided (Block.java and BlockChain.java). The logic found in Task 0 will be reused in Task 1.

The execution of Task 0, a non-distributed stand-alone program, will look like the following interaction. The execution will appear similar to the one below.

Of course, your code - not mine - will produce the console interaction.

### Task 0 Execution

```
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Currupt the chain.
5. Hide the curruption by repairing the chain.
6. Exit
0
Current size of chain: 1
Difficulty of most recent block: 2
Total difficulty for all blocks: 2
Experimented with 2,000,000 hashes.
Approximate hashes per second on this machine: 3067484
Expected total hashes required for the whole chain: 256.000000
Nonce for most recent block: 23
Chain hash: 009CA7F52F76137CE4E909F1AFFF143FFEBFC8E9BB0352C2CE635B6DA56C7533
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the curruption by recomputing hashes.
6. Exit
1
Enter difficulty > 1
4
Enter transaction
Alice pays Bob 100 DSCoin
Total execution time to add this block was 749 milliseconds
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the curruption by recomputing hashes.
6. Exit
1
Enter difficulty > 1
4
Enter transaction
Bob pays Carol 20 DSCoin
Total execution time to add this block was 721 milliseconds
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the curruption by recomputing hashes.
6. Exit
1
Enter difficulty > 1
4
Enter transaction
Carol pays Donna 10 DSCoin
Total execution time to add this block was 198 milliseconds
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the curruption by recomputing hashes.
6. Exit
3
View the Blockchain
{"ds_chain" : [ {"index" : 0,"timeStamp " : "2024-02-16 14:20:10.92","Tx ": "Genesis","PrevHash" : "","nonce" : 23,"difficulty": 2},
{"index" : 1,"timeStamp " : "2024-02-16 14:21:09.493","Tx ": "Alice pays Bob 100 DSCoin","PrevHash" : "009CA7F52F76137CE4E909F1AFFF143FFEBFC8E9BB0352C2CE635B6DA56C7533","nonce" : 66875,"difficulty": 4},
{"index" : 2,"timeStamp" : "2024-02-16 14:21:59.935","Tx ": "Bob pays Carol 20 DSCoin","PrevHash" : "0000C6B27DDF1448992041474D3B2708F76F6D93C226A76DC494EA9540D562F6","nonce" : 65307,"difficulty": 4},
{"index" : 3,"timeStamp " : "2024-02-16 14:22:28.612","Tx ": "Carol pays Donna 10 DSCoin","PrevHash" : "0000980FB682AACB67BC202B0861A6B25D47368D8B2F2D2AA2B197F763DA9D07","nonce" : 14417,"difficulty": 4}
 ], "chainHash":"00003BEBAC2DCD0541FAB71654B61788AC484E2021EF86542D70FACA02E43CBB"}
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the curruption by recomputing hashes.
6. Exit
2
Verifying entire chain
Chain verification: TRUE
Total execution time required to verify the chain was 1 milliseconds
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the curruption by recomputing hashes.
6. Exit
4
Currupt the Blockchain
Enter block ID of block to currupt
2
Enter new data for block 2
Bob pays Tony 30 DSCoin
Block 2 now holds Bob pays Tony 30 DSCoin
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the curruption by recomputing hashes.
6. Exit
3
View the Blockchain
{"ds_chain" : [ {"index" : 0,"timeStamp " : "2024-02-16 14:20:10.92","Tx ": "Genesis","PrevHash" : "","nonce" : 23,"difficulty": 2},
{"index" : 1,"timeStamp " : "2024-02-16 14:21:09.493","Tx ": "Alice pays Bob 100 DSCoin","PrevHash" : "009CA7F52F76137CE4E909F1AFFF143FFEBFC8E9BB0352C2CE635B6DA56C7533","nonce" : 66875,"difficulty": 4},
{"index" : 2,"timeStamp " : "2024-02-16 14:21:59.935","Tx ": "Bob pays Tony 30 DSCoin","PrevHash" : "0000C6B27DDF1448992041474D3B2708F76F6D93C226A76DC494EA9540D562F6","nonce" : 65307,"difficulty": 4},
{"index" : 3,"timeStamp" : "2024-02-16 14:22:28.612","Tx ": "Carol pays Donna 10 DSCoin","PrevHash" : "0000980FB682AACB67BC202B0861A6B25D47368D8B2F2D2AA2B197F763DA9D07","nonce" : 14417,"difficulty": 4}
 ], "chainHash":"00003BEBAC2DCD0541FAB71654B61788AC484E2021EF86542D70FACA02E43CBB"}
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the curruption by recomputing hashes.
6. Exit
2
Verifying entire chain
Chain verification: FALSE
Improper hash on node 2 Does not begin with 0000
Total execution time required to verify the chain was 2 milliseconds
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the curruption by recomputing hashes.
6. Exit
5
Repairing the entire chain
Total execution time required to repair the chain was 2727 milliseconds
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the curruption by recomputing hashes.
6. Exit
2
Verifying entire chain
Chain verification: TRUE
Total execution time required to verify the chain was 0 milliseconds
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the curruption by recomputing hashes.
6. Exit
3
View the Blockchain
{"ds_chain" : [ {"index" : 0,"timeStamp " : "2024-02-16 14:20:10.92","Tx ": "Genesis","PrevHash" : "","nonce" : 23,"difficulty": 2},
{"index" : 1,"timeStamp " : "2024-02-16 14:21:09.493","Tx ": "Alice pays Bob 100 DSCoin","PrevHash" : "009CA7F52F76137CE4E909F1AFFF143FFEBFC8E9BB0352C2CE635B6DA56C7533","nonce" : 66875,"difficulty": 4},
{"index" : 2,"timeStamp " : "2024-02-16 14:21:59.935","Tx ": "Bob pays Tony 30 DSCoin","PrevHash" : "0000C6B27DDF1448992041474D3B2708F76F6D93C226A76DC494EA9540D562F6","nonce" : 28537,"difficulty": 4},
{"index" : 3,"timeStamp " : "2024-02-16 14:22:28.612","Tx ": "Carol pays Donna 10 DSCoin","PrevHash" : "00007A3C0501FC5465326B9CA9FCA2FB9CDEF0B459B3F542BA5B778DC724E278","nonce" : 202928,"difficulty": 4}
 ], "chainHash":"0000F408E8F5C013A4835E6A62D053ADA5D7BFAC921042F5563B8D09E4C55C3F"}
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the curruption by recomputing hashes.
6. Exit
6

```

See the JavaDoc of the main routine in Blockchain.java.  You are asked to experiment and provide some timing
data and analysis. That commentary will be present in the comments of your main routine of Blockchain.java.

**Please use the exact same JSON key names and structure as shown above (case-sensitive)**



- When the user chooses option 1 to add a transaction, their program must (1) ask for the difficulty, (2) ask for the transaction string, (3) actually mine a block with that difficulty, and then print the exact phrase Total execution time to mine this block was X milliseconds. Next, when the user chooses option 2 to verify, their code must check the whole chain and then print Chain verification: TRUE if everything is valid.
Menu printing: Every time the program loops, it must display the menu exactly as written, including punctuation and wording.
If the spacing, periods, or capitalization differ, the test will fail. When the user selects to “View the blockchain,” the program must print a JSON object that includes:
A top-level JSON array named "ds_chain" containing all the blocks.
A top-level string field named "chainHash" with the most recent block’s hash.
The exact order of fields inside the JSON is not important, but the keys must appear in the output.
When a user adds new blocks, each block should be mined with the difficulty they enter and linked to the previous one. If the user chooses to corrupt the chain, the program should let them pick a block and change its data without re-mining it. After that, if the user runs verify, the program must detect the problem and clearly print Chain verification: FALSE. When the user repairs the chain, the program should go back to the first broken block, re-mine it and all blocks after it so the links and hashes are fixed. If the user verifies again, it should now print Chain verification: TRUE.
----

1. The execution (as shown by the console interaction) is correct and includes the same tests as above - using the **same names** and in the **same order**
2. The code is well documented
3. The analysis in the main routine is detailed and clear
Within your comments in the main routine, you must describe how this system behaves as the difficulty level increases. Run some experiments by adding new blocks with increasing difficulties. Describe what you find. Be specific and quote some times.
You need not employ a system clock. You should be able to make clear statements describing the approximate run times associated with addBlock(), isChainValid(), and chainRepair().
4. The code illustrates separation of concerns and good style

## Submission Details 
     Submit just Block.java and BlockChain.java to Gradescope
-----

## Task 1 A Client Server Blockchain Simulator

The client side execution of Task 1 will appear exactly the same as in Task 0. The primary difference will be that, behind the scenes, there will be a client server interaction using JSON over TCP sockets. The blockchain will exist on the server. It will be constructed there and the client will make requests for operations over a TCP socket. The client will be focused on driving the menu driven interaction and communicating with the server on the backend. If the client exits, the server will still handle new requests with the existing blockchain intact.

Here is a TCP server from the Coulouris text. It listens on port 777. Run this before running the client.

```
import java.net.*;
import java.io.*;
import java.util.Scanner;

public class EchoServerTCP {

    public static void main(String args[]) {
        Socket clientSocket = null;
        try {
            int serverPort = 7777; // the server port we are using

            // Create a new server socket
            ServerSocket listenSocket = new ServerSocket(serverPort);

            /*
             * Block waiting for a new connection request from a client.
             * When the request is received, "accept" it, and the rest
             * the tcp protocol handshake will then take place, making
             * the socket ready for reading and writing.
             */
            clientSocket = listenSocket.accept();
            // If we get here, then we are now connected to a client.

            // Set up "in" to read from the client socket
            Scanner in;
            in = new Scanner(clientSocket.getInputStream());

            // Set up "out" to write to the client socket
            PrintWriter out;
            out = new PrintWriter(new BufferedWriter(new OutputStreamWriter(clientSocket.getOutputStream())));

            /*
             * Forever,
             *   read a line from the socket
             *   print it to the console
             *   echo it (i.e. write it) back to the client
             */
            while (true) {
                String data = in.nextLine();
                System.out.println("Echoing: " + data);
                out.println(data);
                out.flush();
            }

        // Handle exceptions
        } catch (IOException e) {
            System.out.println("IO Exception:" + e.getMessage());

        // If quitting (typically by you sending quit signal) clean up sockets
        } finally {
            try {
                if (clientSocket != null) {
                    clientSocket.close();
                }
            } catch (IOException e) {
                // ignore exception on close
            }
        }
    }
}

```

Here is a TCP client from the Coulouris text. It makes visits to port 7777. Run this after running the server.

```
import java.net.*;
import java.io.*;

public class EchoClientTCP {

    public static void main(String args[]) {
        // arguments supply hostname
        Socket clientSocket = null;
        try {
            int serverPort = 7777;
            clientSocket = new Socket(args[0], serverPort);

            BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
            PrintWriter out = new PrintWriter(new BufferedWriter(new OutputStreamWriter(clientSocket.getOutputStream())));


            BufferedReader typed = new BufferedReader(new InputStreamReader(System.in));
            String m;
            while ((m = typed.readLine()) != null) {
                out.println(m);
                out.flush();
                String data = in.readLine(); // read a line of data from the stream
                System.out.println("Received: " + data);
            }
        } catch (IOException e) {
            System.out.println("IO Exception:" + e.getMessage());
        } finally {
            try {
                if (clientSocket != null) {
                    clientSocket.close();
                }
            } catch (IOException e) {
                // ignore exception on close
            }
        }
    }
}

```

Your Task 1 client will be named ClientTCP.java ad your Task1 server will be named ServerTCP.java.

You are required to design and use two JSON messages types - a message to encapsulate
requests from the client and a message to encapsulate responses from the server. The server side display will
show each request message (received from the client -in JSON) and each response message (being sent to the client - in JSON).

In Project 2, you were provided with the structure for request and response messages. In Project 3, we are asking that you design your own request and response messages.

You should have a class named RequestMessage and a class named ResponseMessage to encapsulate the JSON data. You need to include these classes in your submission. You will use the RequestMessage class on both the client and the server. And, you will use the ResponseMessage class on both the client and the server.

Use the following four labels in your single PDF:

**Task 1 Client Side Execution**

Copy and paste your client side console. This will appear as in Task 0.

**Task 1 Server Side Execution**

Here, we will see the request and response messages in JSON format. We will also see the number of
blocks on the chain after each visit.

```
Blockchain server running

We have a visitor
THE JSON REQUEST MESSAGE IS SHOWN HERE
THE JSON RESPONSE MESSAGE IS SHOWN HERE
Number of Blocks on Chain == XX.

We have a visitor
THE JSON REQUEST MESSAGE IS SHOWN HERE
THE JSON RESPONSE MESSAGE IS SHOWN HERE
Number of Blocks on Chain == XX.

We have a visitor
THE JSON REQUEST MESSAGE IS SHOWN HERE
THE JSON RESPONSE MESSAGE IS SHOWN HERE
Number of Blocks on Chain == XX.

etc.

```

**Task 1 Client Source Code in the file ClientTCP.java.**

Include all client side source code clearly labelled. This includes the RequestMessage and ResponseMessage classes.

**Task 1 Server Source Code in the file ServerTCP.java.**

Include all server side source code clearly labelled. This includes the RequestMessage and ResponseMessage classes. These will be the same classes as found on the client side.

**Task 1 Grading Rubric 40 Points**

Rubric:
1. The execution is correct and includes the same tests as above - in the same order. A client server architecture based on JSON messages over TCP sockets is used. 15 points.
2. The JSON request and response messages are well designed. 5 Points
3. The JSON message being sent to the server is well designed and is represented in RequestMessage.java: 5 Points
4. The JSON message being sent from the server to the client is well designed and is represented in ResponseMessage.java: 5 Points.
5. Separation of concerns is well done with a proxy design. : 5 points.
6. The single PDF file includes sections correctly labelled: 5 Points.

-----
## Task 2 RSA Digital Signatures

## Task 2 illustrates client authentication using signatures. Name the IntelliJ project "Project3Task2".

Before starting this task, study the three programs below. RSAExample.java shows how you can generate RSA keys in Java. ShortMessageSign.java and ShortMessageVerify.java shows you how you can sign and veify the signature on very small messages.

This Task is modeled after the way an Ethereum blockchain client signs requests. In Ethereum, each request to execute a transaction costs gas. That is, the sender has to pay some amount of Eth for resources used. In Ethereum, each client that wants to to execute a transactions must provide an identifier so that a deduction can be made from his or her account.

Make the following modifications to your work in Task 1.

0. Rename the files and call them "SigningClientTCP.java" and "VerifyingServerTCP.java".

1. As before, the client will be interactive and menu driven. It will transmit requests to the server, along with an ID computed in 3 below, and provide an option to exit.

2. We want to send signed request from the client. Each time the client program runs, it will create new RSA public and private keys and **display** these keys to the user. See the RSAExample.java program below for guidance on how to build these keys. It is fine to use the code that you find in RSAExample.java (with citations, of course). After the client (SigningClientTCP.java) creates and displays these keys, it interacts with the user and the server (VerifyingServerTCP).

3. The client&#39;s ID will be formed by taking the least significant 20 bytes of the hash of the client&#39;s public key. Note: an RSA public key is the pair e and n. Prior to hashing, you will combine these two integers with concatenation. The ID is computed in the client code. As in Bitcoin or Ethereum, the user's ID is derived from the public key.

4. The client will also transmit its public key with each request. Again, note that this key is a combination of e and n. These values will be transmitted in the clear and will be used by the server to verify the signature.

5. Finally, the client will sign each request. So, by using its private key (d and n), the client will encrypt the hash of the message it sends to the server. The signature will be added to each request. It is very important that the big integer created with the hash (before signing) is positive. RSA does not work with negative integers. See details in the code of ShortMessageSign.java and ShortMessageVerify.java below. You may use this code if cited.

6. The server will make two checks before servicing any client request. First, does the public key (included with each request) hash to the ID (also provided with each request)? Second, is the request properly signed? If both of these are true, the request is carried out on behalf of the client. The server will behave as it did in Task 1. Otherwise, the server returns the message &quot;Error in request&quot;.

7. By studying ShortMessageVerify.java and ShortMessageSign.java you will know how to compute a signature. Your solution, however, will not use the short message approach as exemplified there. Note that we are not using any Java crypto API&#39;s that abstract away the details of signing.

8. We will use SHA-256 for our hash function h(). To clarify further:

The client will send the id: last20BytesOf(h(e+n)), the public key: e and n in the clear, the request in the clear, and the signature E(h(all prior tokens),d). The signature is thus an encrypted hash. It is encrypted
using d and n - the client&#39;s private key. E represents standard RSA encryption. The function h(e+n) is the SHA-256 hash of e concatenated with n.

During one client session, the ID will always be the same. If the client quits and restarts, it will have a new ID. The server is left running and survives client restarts.

As before, use a **proxy design** to encapsulate the communication code.

Produce a screen shot (or copy and paste) illustrating a successful execution and submit the interaction in the single pdf file as described at the end of this document. The screen shot (or copy and paste) will show three different clients interacting with the server using three distinct ID&#39;s.

When signing a message using JSON, the normal practice is to construct a JSON message like this:

{"NameOfField1":"Value1", "NameOfField2":"Value2","signature":"value of signature"}

The signature value is an encrypted hash of the concatenation of all of the values (except the signature value) and does not include the names. This should make signature creation and verification easier to do. So, in this example, you would really only sign "Value1" + "Value2". You would not include "NameOfField1" or "NameOfField2" in the computation of the signature.


:checkered_flag:**On your single pdf, make a copy of your client side code and label it clearly as "Project3Task2SigningClient".**

:checkered_flag:**On your single pdf, make a copy of your server side code and label it clearly as "Project3Task2VerifyingServer".**

:checkered_flag:**Take a screenshot (or copy and paste) of your client console screen. Show a single client interacting with the server using the keys generated when the client code is run. All of the client's key material must be displayed on the client side console. Display the private key (d and n) and the public key (e and n). The client will perform a few operations on the server. Each request will be signed. On your single pdf, label this screenshot as "Project3Task2ClientConsole".**

:checkered_flag:**Take a screenshot of your server console screen. It will show each visitor's public key material (e and n) and whether or not the signature is verified. On your single pdf, label this screenshot as "Project3Task2ServerConsole". You will show three different visitors who are all verified.**

**Task 2 Grading Rubric 20 Points**

Rubric:
1. All messages are signed by the client and verified on the server. 10 points.
3. The JSON message being sent to the server is well designed and is represented in RequestMessage.java: 2 Points
4. The JSON message being sent from the server to the client is well designed and is represented in ResponseMessage.java: 3 Points.
5. Separation of concerns is well done with a proxy design. : 2 points.
6. The single PDF file includes sections correctly labelled: 3 Points.


RSAExample.java - Key generation and sample encryption and decryption

```
/* Demonstrate RSA in Java using BigIntegers */

package edu.cmu.andrew.mm6;

import java.io.UnsupportedEncodingException;
import java.math.BigInteger;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Random;

/**
 *  RSA Algorithm from CLR
 *
 * 1. Select at random two large prime numbers p and q.
 * 2. Compute n by the equation n = p * q.
 * 3. Compute phi(n)=  (p - 1) * ( q - 1)
 * 4. Select a small odd integer e that is relatively prime to phi(n).
 * 5. Compute d as the multiplicative inverse of e modulo phi(n). A theorem in
 *    number theory asserts that d exists and is uniquely defined.
 * 6. Publish the pair P = (e,n) as the RSA public key.
 * 7. Keep secret the pair S = (d,n) as the RSA secret key.
 * 8. To encrypt a message M compute C = M^e (mod n)
 * 9. To decrypt a message C compute M = C^d (mod n)
 */

public class RSAExample {

        public static void main(String[] args) {
                // Each public and private key consists of an exponent and a modulus
                BigInteger n; // n is the modulus for both the private and public keys
                BigInteger e; // e is the exponent of the public key
                BigInteger d; // d is the exponent of the private key

                Random rnd = new Random();

                // Step 1: Generate two large random primes.
                // We use 400 bits here, but best practice for security is 2048 bits.
                // Change 400 to 2048, recompile, and run the program again and you will
                // notice it takes much longer to do the math with that many bits.
                // To sign, say, a 256 bit hash, 2048 bits is recommended.
                BigInteger p = new BigInteger(400, 100, rnd);
                BigInteger q = new BigInteger(400, 100, rnd);

                // Step 2: Compute n by the equation n = p * q.
                n = p.multiply(q);

                // Step 3: Compute phi(n) = (p-1) * (q-1)
                BigInteger phi = (p.subtract(BigInteger.ONE)).multiply(q.subtract(BigInteger.ONE));

                // Step 4: Select a small odd integer e that is relatively prime to phi(n).
                // By convention the prime 65537 is used as the public exponent.
                e = new BigInteger("65537");

                // Step 5: Compute d as the multiplicative inverse of e modulo phi(n).
                d = e.modInverse(phi);

                System.out.println(" e = " + e);  // Step 6: (e,n) is the RSA public key
                System.out.println(" d = " + d);  // Step 7: (d,n) is the RSA private key
                System.out.println(" n = " + n);  // Modulus for both keys

                // Encode a simple message. For example the letter 'A' in UTF-8 is 65
                BigInteger m = new BigInteger("65");

                // Step 8: To encrypt a message M compute C = M^e (mod n)
                BigInteger c = m.modPow(e, n);

                // Step 9: To decrypt a message C compute M = C^d (mod n)
                BigInteger clear = c.modPow(d, n);
                System.out.println("Cypher text = " + c);
                System.out.println("Clear text = " + clear); // Should be "65"

                // Step 8 Encrypt the string 'RSA is way cool.'
                String s = "RSA is way cool.";
                m = new BigInteger(s.getBytes()); // m is the original clear text
                c = m.modPow(e, n);     // Do the encryption, c is the cypher text

                // Step 9 Decrypt...
                clear = c.modPow(d, n); // Decrypt, clear is the resulting clear text
                String clearStr = new String(clear.toByteArray());  // Decode to a string

                System.out.println("Cypher text = " + c);
                System.out.println("Clear text = " + clearStr);

        }
}

```

ShortMessageSign.java - Signing

```
import java.math.BigInteger;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Scanner;

/** ShortMessageSign.java provides capabilities to sign
 *  very short messages. These messages are 4 hex digits.
 *  ShortMessageSign has three private members: RSA e,d and n.
 *  These are all very small java BigIntegers. ShortMessageSign is
 *  only used for instructional purposes.
 *
 *  For signing: the ShortMessageSign object is constructed with RSA
 *  keys (e,d,n). These keys are not created here but are passed in by the caller.
 *  Then, a caller can sign a message - the string returned by the sign
 *  method is evidence that the signer has the associated private key.
 *  After a message is signed, the message and the string may be transmitted
 *  or stored.
 *  The signature is represented by a base 10 integer.
 */


public class ShortMessageSign {

    private BigInteger e,d,n;

    /** A ShortMessageSign object may be constructed with RSA's e, d, and n.
     *  The holder of the private key (the signer) would call this
     *  constructor. Only d and n are used for signing.
     */
    public ShortMessageSign(BigInteger e, BigInteger d, BigInteger n) {
        this.e = e;
        this.d = d;
        this.n = n;
    }

    /**
     * Signing proceeds as follows:
     * 1) Get the bytes from the string to be signed.
     * 2) Compute a SHA-1 digest of these bytes.
     * 3) Copy these bytes into a byte array that is one byte longer than needed.
     *    The resulting byte array has its extra byte set to zero. This is because
     *    RSA works only on positive numbers. The most significant byte (in the
     *    new byte array) is the 0'th byte. It must be set to zero.
     * 4) Create a BigInteger from the byte array.
     * 5) Encrypt the BigInteger with RSA d and n.
     * 6) Return to the caller a String representation of this BigInteger.
     * @param message a sting to be signed
     * @return a string representing a big integer - the encrypted hash.
     * @throws Exception
     */
    public String sign(String message) throws Exception {

        // compute the digest with SHA-256
        byte[] bytesOfMessage = message.getBytes("UTF-8");
        MessageDigest md = MessageDigest.getInstance("SHA-256");
        byte[] bigDigest = md.digest(bytesOfMessage);

        // we only want two bytes of the hash for ShortMessageSign
        // we add a 0 byte as the most significant byte to keep
        // the value to be signed non-negative.
        byte[] messageDigest = new byte[3];
        messageDigest[0] = 0;   // most significant set to 0
        messageDigest[1] = bigDigest[0]; // take a byte from SHA-256
        messageDigest[2] = bigDigest[1]; // take a byte from SHA-256

        // The message digest now has three bytes. Two from SHA-256
        // and one is 0.

        // From the digest, create a BigInteger
        BigInteger m = new BigInteger(messageDigest);

        // encrypt the digest with the private key
        BigInteger c = m.modPow(d, n);

        // return this as a big integer string
        return c.toString();
    }

    public static void main(String args[]) throws Exception {

        // Test driver for ShortMessageSign

        // Since we are signing only very short messages, we can generate some really small keys.
        // The keys were generated by the RSA algorithm
        // p and q were 20 bits each.
        // In practice, the keys would be much larger.

        BigInteger e = new BigInteger("65537");
        BigInteger d = new BigInteger("5420920152787448033");
        BigInteger n = new BigInteger("9013594933187057813");

        ShortMessageSign sov = new ShortMessageSign(e,d,n);

        Scanner sc = new Scanner(System.in);

        // Get some data to sign
        System.out.println("Enter data to be signed (at most 4 hex digits)");
        System.out.println("All data will be converted to lower case");
        String inStr = sc.nextLine();
        if(inStr.length() != 4) {
            System.out.println("Error in input " + inStr);
            return;
        }
        inStr = inStr.toLowerCase();

        String signedVal = sov.sign(inStr);
        System.out.println("Signed Value");
        System.out.println(signedVal);


    }
    // From Stack overflow
    public static byte[] hexStringToByteArray(String s) {
        int len = s.length();
        byte[] data = new byte[len / 2];
        for (int i = 0; i < len; i += 2) {
            data[i / 2] = (byte) ((Character.digit(s.charAt(i), 16) << 4)
                    + Character.digit(s.charAt(i+1), 16));
        }
        return data;
    }
}

```
ShortMessageVerify - Signature verification

```
import java.math.BigInteger;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Scanner;

/** ShortMessageVerify.java provides capabilities to verify
 *  very short messages. These messages are 4 hex digits.
 *  ShortMessageVerify has two private members: RSA e and n.
 *  These are all very small java BigIntegers. ShortMessageVerify is
 *  only used for instructional purposes.
 *
 *
 *  For verification: the object is constructed with keys (e and n). The verify
 *  method is called with two parameters - the string to be checked and the
 *  evidence that this string was indeed manipulated by code with access to the
 *  private key d.
 *  The message that is signed or verified is 4 hex digits.
 *  The signature is represented by a base 10 integer.
 */


public class ShortMessageVerify {

    private BigInteger e,n;

    /** For verifying, a SignOrVerify object may be constructed
     *   with a RSA's e and n. Only e and n are used for signature verification.
     */
    public ShortMessageVerify(BigInteger e, BigInteger n) {
        this.e = e;
        this.n = n;
    }

    /**
     * Verifying proceeds as follows:
     * 1) Decrypt the encryptedHash to compute a decryptedHash
     * 2) Hash the messageToCheck using SHA-256 (be sure to handle
     *    the extra byte as described in the signing method.)
     * 3) If this new hash is equal to the decryptedHash, return true else false.
     *
     * @param messageToCheck  a normal string (4 hex digits) that needs to be verified.
     * @param encryptedHashStr integer string - possible evidence attesting to its origin.
     * @return true or false depending on whether the verification was a success
     * @throws Exception
     */
    public boolean verify(String messageToCheck, String encryptedHashStr)throws Exception  {

        // Take the encrypted string and make it a big integer
        BigInteger encryptedHash = new BigInteger(encryptedHashStr);
        // Decrypt it
        BigInteger decryptedHash = encryptedHash.modPow(e, n);

        // Get the bytes from messageToCheck
        byte[] bytesOfMessageToCheck = messageToCheck.getBytes("UTF-8");

        // compute the digest of the message with SHA-256
        MessageDigest md = MessageDigest.getInstance("SHA-256");

        byte[] messageToCheckDigest = md.digest(bytesOfMessageToCheck);

        // messageToCheckDigest is a full SHA-256 digest
        // take two bytes from SHA-256 and add a zero byte
        byte[] extraByte = new byte[3];
        extraByte[0] = 0;
        extraByte[1] = messageToCheckDigest[0];
        extraByte[2] = messageToCheckDigest[1];

        // Make it a big int
        BigInteger bigIntegerToCheck = new BigInteger(extraByte);

        // inform the client on how the two compare
        if(bigIntegerToCheck.compareTo(decryptedHash) == 0) {

            return true;
        }
        else {
            return false;
        }
    }


    public static void main(String args[]) throws Exception {

        // Test driver for ShortMessageVerify

        // ShortMessageVerify may use some really small keys
        // The keys were generated by the RSA algorithm
        // p and q were 20 bits each
        BigInteger e = new BigInteger("65537");

        BigInteger n = new BigInteger("9013594933187057813");

        ShortMessageVerify verifySig = new ShortMessageVerify(e,n);

        Scanner sc = new Scanner(System.in);

        // Check an existing signature

        System.out.println("Enter hash that was signed (4 hex digits)");
        System.out.println("All data will be converted to lower case");
        String data = sc.nextLine();
        if(data.length() != 4) {
            System.out.println("Invalid input");
            System.exit(0);
        }
        data = data.toLowerCase();
        System.out.println("Enter an integer representing the signature");
        String sig = sc.nextLine();
        if(verifySig.verify(data, sig)) {
            System.out.println("Valid signature");
        }
        else {
            System.out.println("invalid signature");
        }

    }
    // from Stack overflow
    public static byte[] hexStringToByteArray(String s) {
        int len = s.length();
        byte[] data = new byte[len / 2];
        for (int i = 0; i < len; i += 2) {
            data[i / 2] = (byte) ((Character.digit(s.charAt(i), 16) << 4)
                    + Character.digit(s.charAt(i+1), 16));
        }
        return data;
    }
}
```


**Project 3 Submission Requirements**

Name your three IntelliJ project folders: Project3Task0, Project3Task1, and Project3Task2.

For each IntelliJ project, File->Export Project->To Zip... each. You must export in this way and NOT just zip the IntelliJ project folders.

You should now have three zip files:
* Project3Task0.zip
* Project3Task1.zip
* Project3Task2.zip

You will also have a single pdf named Project3.pdf.

Create a new empty folder named with your Andrew id (**very important**). Put the four files above into this new folder. Zip that folder, and submit it to Canvas.

**The file named YourAndrewID.zip contains three zip files and a single pdf file:**
* Project3Task0.zip
* Project3Task1.zip
* Project3Task2.zip
* Project3.pdf







