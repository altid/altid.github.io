---
type: section
---

# Walkthrough Of A Basic Setup

A basic Altid setup involves three pieces:
 - One or more Altid services (See [list of services](#list-of-services) below)
 - One or more transport servers
 - A client to talk to the server

(See [list of servers and clients](#list-of-servers-and-clients) below)

## Getting The Pieces

For this example, we'll use ircfs, 9p-server, and its example client. These are all built with the Go programming language, which is currently required for a full Altid setup.

Go has simple tooling for fetching source for, and building applications. Grabbing each, and building an executable can be done with just the following steps:

```
# We'll work in our own directory for now

mkdir altid && cd altid 

go get github.com/altid/ircfs
go build -o ircfs github.com/altid/ircfs

go get github.com/altid/9p-server
go build -o 9p-server github.com/altid/9p-server

# We'll grab one of the shell clients, which are adequate for testing
git clone https://github.com/altid/shell-client


```

This will create three binaries in your current directory. Later, you can simply `mv` these to somewhere in your PATH; or run the `go install` command for each binary.

## Modifying Your altid/config

Most Altid services use a file called altid/config for their setup. It has a simple format:

```
# Example altid/cfg

service=irc server=irc.freenode.net port=6667 ssl=none
	log=/home/halfwit/logs
	# user details here

service=docs 
	log=/home/halfwit/logs/

service=test
	log=none
```

For a given service, there will always be at least one flag, `service=`.

`log=` is present in each section here, but it is also allowable to set it globally, as follows

```
# Example2 altid/config

service=irc server=irc.freenode.net port=6667 ssl=none
	# user details here

service=docs

service=test

```

Add an entry for each service you wish to use to your altid/config example entries are found on the main page of each service listed below.

## Running Altid

Once your altid/config is set up, you're mostly good to go. Start up your service:

`./ircfs &`

This will create a set of directories, one for each service, by default in /tmp/altid.

```
> ls /tmp/altid
/tmp/altid/irc/
/tmp/altid/otherservice/
```

And it's ready to go! Now, these folders are interesting in their own right, but not particularily useful. To interact with these, you need an Altid server, and a client that can connect to it. For our example, we'll use 9p-server, and the simple example client that ships with it.

## Server Set Up

`9p-server &` will start the 9p server in the background, listening for connections. The default port for connections is `:564`. (To modify this, see [Using listen_address](using-listen-address.md)

## Client

To connect to your running server is simple.


```
cd shell-client/
altid-sh localhost
# Or
#altid-rc localhost
```

Or if on a remote machine, and your 9p-server is on a machine with the ip address 192.168.1.104

```
altid-sh 192.168.1.104

```
The default port it connects to is `:564`, make sure this matches if you've changed the listening port on 9p-server.

## Commands

As a side note, there are a number of commands you can run from an Altid client. The whole list depends on the service you are connected to, but the following will always be available:

 - `/buffer <buffer>`: change the current viewed buffer
 - `/open <buffer>`: open a buffer
   - on IRC, this will connect to a channel. In docfs, it will open the named document at the path sent it. On smsfs, it will start a conversation window with the person at that number
 - `/close <buffer>`: closes the buffer, optionally disconnecting any network-based session
 - `/quit`: Ends the client session

## Doing Things

In your client, type `/tabs` to see a list of currently opened buffers you can view.

Now, to change to one of those buffers type `/buffer <the tab you wish to view>`

This should cause the main document body of the tab you switched to, to be displayed. In the case of a service which constantly outputs new data, such as ircfs or discordfs, you'll see each new line as they come in for that buffer. Additionally, try typing `/title` to see the channel topic.

If you're connected to an IRC channel, type some text to your friends, and when you're finished, type `/quit` to exit the client.

## List of Services

 - [ircfs](https://github.com/altid/ircfs): Connect and chat people on IRC networks
 - [docfs](https://github.com/altid/docfs): Read PDF and EPUB files
 - [testfs](https://github.com/altid/testfs): Open text files to view and modify text

### Services Planned
 - gamefs: Play retro games!
 - httpfs: Simple web docment viewer
 - discordfs: Connect and chat with people on Discord
 - mailfs: Read, write, and view emails
 - newsfs: RSS aggregator and viewer
 - smsfs: Read, write, and view sms messages
 - undofs: Edit documents with unlimited undo/redo, using the integrated Sam command language 
 - videofs: Search and watch video streams, pause and switch devices

## List of Servers and Clients

 - [9p server](https://github.com/altid/9p-server): Uses 9p from Plan9
 - [Shell-based clients](https://github.com/altid/shell-client)
 - [linux-client](https://github.com/altid/linux-client): A compile-time plugin based client which allows you to select which server you want to connect to, what type of input you prefer to use, and the graphics drawing library you prefer for how you're using the client.

### Servers Planned

 - html5 server: Access altid via a web browser
 - ssh server: Access altid through an ssh tunnel

### Clients Planned

 - plan9-client: Simple client for use on the Plan9 operating system
 - plan9-nuklear: A variant of the plan9 client using the Nuklear immediate mode UI library
 - Android/iOS mobile client: Touch-based interface to altid
 - Kodi client: linux-client based Kodi plugin
