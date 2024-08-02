### Socket Class Methods

The socket class methods are the special functions that let us implement the socket programming. The program that has to be written in order to bring the functionalities of [socket programming uses](https://www.educba.com/socket-programming-in-c-plus-plus/) the predefined socket functions. These functions consist of the statements that perform the actual role in socket programming. Below are some of the socket functions.

- **Socket_accept:** This is one of the very common socket functions used to accept a socket connection. The primary role of this function is to let the connection accepted whenever a request hits.
- **Socket_addrinfo_bind:** This function is used to add the provided information to the socket. The information accepted has to be assigned to the socket to facilitate its implementation.
- **Socket_clear_error:** This function is used to clear the error that is on the socket. In addition to that, this function also clears the error on the last code.
- **Socket_close:** As the name states, this function is used to close the resource that belongs to the socket.
- **Socket_connect:** This method is used to create a socket connection. In socket programming, the program begins with the establishment of the connection, which can be done using this function.
- **Socket_create:** This method is concerned with the creation of the socket. The socket created using this method works as the endpoint of the connection.
- **Socket_create_listen:** This function is used to get the socket to open the specified port that accepts the connection. As the name states, it helps in opening the socket for listening.
- **Socket_create_pair:** This method is usually used in the application that needs to bring the complex part of [socket programming in use](https://www.educba.com/socket-programming-in-python/). It helps in the creation of the indistinguishable sockets, and those are stored in the array.
- **Socket_get_option:** This method is used to get the options for the socket. A socket is comprised of several options that have to be used in accordance with the application. By using this method, we can get all those options that a socket has.
- **Socket_getsockname:** This method is used to query the local region of the selected socket, and in return, it may get the details related to the host/port or the [Unix filesystem path](https://www.educba.com/unix-file-system/). Whatever outcome it gets is totally dependent on the type.

### Socket Client Example

This section will see the code that will be used to implement the client-side socket programming. The example mentioned below will be having the post and the host details that will be used to create the socket connection. One the connection is established, it exchanges some of the messages and expects a response from the server.

```php
<?php
$port_number    = 1230;
$IPadress_host    = "127.0.0.1";
$hello_msg= "This is server";
echo "Hitting the server :".$hello_msg;
$socket_creation = socket_create(AF_INET, SOCK_STREAM, 0) or die("Unable to create connection with socket\n");
$server_connect = socket_connect($socket_creation, $IPadress_host , $port_number) or die("Unable to create connection with server\n");
socket_write($socket_creation, $hello_msg, strlen($hello_msg)) or die("Unable to send data to the  server\n");
$server_connect = socket_read ($socket_creation, 1024) or die("Unable to read response from the server\n");
echo "Message from the server :".$server_connect;
socket_close($socket_creation);
?>
```

In the above example, the port number is 1230 in which the program tries to connect. The IP address of the host will be the localhost’s IP. If anyone is willing to interact with the remote server, they can mention the server’s IP address. Then the message will be sent to the server that will be shown on the response page. The socket creation will be processed afterward. In this program, there is a proper mechanism to handle the error by using the die method. If anything went wrong, in that case, the die method gets revoked, and the message given in that pops up.

### Socket Server Example

The example detailed in this section will be having the PHP codes that will be leveraged to implement the socket programming at the server-side. The details of the IP and the port number used in the last example will remain the same in this example as well. This example’s main difference will make the core difference that separates it from the client-side socket programming language. Lets process to understand the PHP code for server-side socket programming.

```php
<?php
$port_number    = 1230;
$IPadress_host    = "127.0.0.1";
set_time_limit(0);
$socket_creation = socket_create(AF_INET, SOCK_STREAM, 0) or die("Unable to create socket\n");$socket_outcome = socket_bind($socket_creation, $IPadress_host , $port_number ) or die("Unable to bind to socket\n");
$socket_outcome = socket_listen($socket_creation, 3) or die("Unable to set up socket listener\n");
$socketAccept = socket_accept($socket_creation) or die("Unable to accept incoming connection\n");
$data = socket_read($socketAccept, 1024) or die("Unable to read input\n");
$data = trim($data);
echo "Client Message : ".$data;
$outcome = strrev($data) . "\n";
socket_write($socketAccept, $outcome, strlen ($outcome)) or die("Unable to  write output\n");
socket_close($socketAccept);
socket_close($socket_creation);
?>
```

In the above example, the program has been developed to work in the localhost. The IP address mentioned here belongs to the localhost, and the port number can run the TCP and UDP service on that. The initial step is always the creation of the socket, as it is something that will be used throughout the program. Later the socket has been bonded with the specified values, which will help in functioning. The methods used in this program have a predefined meaning that can be used for a specific purpose. Once everything goes well, the program will work accordingly and will close the socket connection eventually.

### Conclusion – Socket Programming in PHP

The socket programming language is used to let the application work on the server and the client model. This approach of programming lets us establish the connection between the server and the client so that the exchange of the data could be facilitated. To make the socket programming easy and convenient, [PHP has provided](https://www.educba.com/php-constants/) predefined methods where all the methods have some unique tasks assigned to them.