# PROGRAMMING-POE-PART-2
=====================================================
CHATAPP & QUICKCHAT
=====================================================

Author: Ompha Maluma

A console-based Java application with two parts:

1. ChatApp   - user registration and login with input validation.
2. QuickChat - once logged in, the user can write, send, discard or
               store text messages, and see a summary at the end.

Stored messages are saved to a JSON file (messages.json).


-----------------------------------------------------
1. FEATURES
-----------------------------------------------------

Registration and login
  - Captures first name, last name, username, password and cellphone number.
  - Validates each input and asks again until it is valid.
  - Login retries until the username and password match the registered ones.

  Validation rules:
    Username .......... Contains an underscore and is no more than
                        5 characters long.
    Password .......... At least 8 characters, with a capital letter,
                        a number and a special character.
    Cellphone number .. International format: +27 followed by exactly
                        9 digits.

QuickChat menu
    1. Send Messages
    2. Show recent sent Messages   (coming soon)
    3. Quit

When sending messages, the user enters how many messages to send, then
for each message:

  1. Enters a recipient cell number (+27 followed by 9 digits).
  2. Enters the message text (maximum 250 characters; the program reports
     how many characters are over the limit).
  3. The program generates a Message ID and a Message Hash.
  4. The user chooses what to do with the message:

       Option 1 - Send     Result: "Message successfully sent."
       Option 2 - Discard  Result: "Press 0 to delete the message."
       Option 3 - Store    Result: "Message successfully stored."
                           (saved to messages.json)

After all messages are processed, a summary shows the total number of
messages sent, stored and discarded, plus the details of every sent and
stored message.

Message ID and hash
  - Message ID: 10 randomly generated digits.
  - Message Hash: <first two digits of the ID>:<message number>:<FIRSTWORD><LASTWORD>
    in upper case, with punctuation removed.

  Example: message number 0, an ID starting with 00, and the text
  "Hi Mike, can you join us for dinner tonight?" give the hash
  00:0:HITONIGHT

JSON storage
  Stored messages are added to messages.json in the folder the program
  runs from. Messages already in the file are kept. Each entry contains:
  messageID, message, messageHash, recipient and messageNumber.


-----------------------------------------------------
2. PROJECT STRUCTURE
-----------------------------------------------------

  src/main/java/com/mycompany/chatapp/
      ChatApp.java      Main program: registration, login, QuickChat menu
      LogIn.java        Registration and login validation
      Message.java      Message creation, validation, send/discard/store,
                        summary

  src/test/java/com/mycompany/chatapp/
      ChatAppTest.java  Unit tests for LogIn
      MessageTest.java  Unit tests for Message

  pom.xml               Maven configuration and dependencies


-----------------------------------------------------
3. REQUIREMENTS
-----------------------------------------------------

  - Java (built with Java 25; Java 11 or newer is needed for
    Files.readString and Files.writeString)
  - Maven
  - NetBeans (or any Java IDE)

Dependencies (in pom.xml):
  - Gson 2.11.0 (com.google.code.gson:gson) - reads and writes the JSON file
  - JUnit Jupiter 5.10.2 (org.junit.jupiter:junit-jupiter, test scope)
    - unit testing


-----------------------------------------------------
4. HOW TO RUN
-----------------------------------------------------

In NetBeans:
  1. Open the project (File > Open Project).
  2. Right-click the project and choose "Clean and Build". This downloads
     the dependencies.
  3. Run ChatApp.java (right-click > Run File, or press Shift + F6).
  4. Type your answers in the Output window and press Enter after each one.

From the command line:
  mvn clean package
  Then run the class com.mycompany.chatapp.ChatApp from your IDE, or with
  the java command and the compiled classes on the classpath.


-----------------------------------------------------
5. RUNNING THE UNIT TESTS
-----------------------------------------------------

In NetBeans, right-click ChatAppTest.java or MessageTest.java and choose
"Test File", or right-click the project and choose "Test".

From the command line:
  mvn test

The tests cover:
  - LogIn: username, password and cellphone validation, their success and
    failure messages, login success and failure, and registration.
  - Message: message length (including exactly 250 characters), recipient
    validation, hash generation (including a loop over several messages),
    Message ID creation, the send/discard/store options, adding to the
    JSON file, and the totals and summary.

The message tests write to a temporary folder, so your real messages.json
is never changed by the tests.


-----------------------------------------------------
6. NOTES AND LIMITATIONS
-----------------------------------------------------

  - Menu options are read as numbers. If you type letters at a number
    prompt, the program asks again.
  - Menu option 2 ("Show recent sent Messages") is not implemented yet and
    shows "Coming soon".
  - Messages are only kept in memory while the program runs, apart from
    those you choose to store in messages.json.

