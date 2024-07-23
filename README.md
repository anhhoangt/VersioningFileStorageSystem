# VersioningFileStorageSystem
Most programs require persistent storage of data either in a file or in a database. Most operating systems provides a file system to allow writing and reading of bytes from a storage device. However, those files are written to a local storage device so sharing and versioning are difficult. So, some programs use a remote file system with versioning.

This project is a server-based versioning file storage system that allows any program to open a TCP connection through a socket (a kind of file in Unix), request file services from a server (reading a file, writing to a file, creating a folder, get information about a file or folder, and deleting a file or folder).

WRITE command:
Allows you to send a command from a client to the server for writing a file from the client's file system to the server. The request needs include some kind of command (e.g., WRITE) and a path to a local file name (e.g., folder/foo.txt) and the path and name of the file on the server. If the remote file or path is omitted, use the values for the first argument.

% rfs WRITE local-file-path remote-file-path 

GET command:
Retrieves a new file from the remote file system, e.g., GET folder/path/file.txt, and writes the data read from the socket to a local file. If the local file path or name (the third command line argument) is omitted, use current folder.

% rfs GET remote_folder/remote_file.html local_folder/local_file.html

RM command:
Deletes a file or folder in the remote file system, e.g., RM folder/path/file.txt, and returns some code or indication about its success status.

% rfs RM folder/somefile.txt

when a WRITE request writes to the same remote file name more than once, that the prior version is not simply overwritten but saved as an older version

How To Run: 
1. start the server program, i.e., rfserver &
2. server waits on a port (whatever you set it, let's say 2023) for clients to connect
3. you do: rfs LS foo/bar.c
4. in the multi-threaded version, a thread is spawned to handle the client's request
5. rfs (the client) connects to the server on port 2023 and send it the string LS foo/bar.c
6. server reads from the socket and gets the string LS foo/bar.c
7. it interprets the command: LS and then calls some handling function, perhaps via a table of pointers to handler functions
8. in that function you get the info and versions for the file (where and how is up to you)
9. server returns the information to the client by writing to the socket; format in whatever way you like
10. client gets the information, parses it, and then printf's it to the console
11. clients (get) disconnect/closes the socket
12. server waits for next client
You turn off the server by killing the process... or you could have another command that stops the server, e.g., fget STOP -- if the server gets that command it calls exit(0) to exit the process after closing the socket.

