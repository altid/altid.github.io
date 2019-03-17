# Walkthrough Of A Basic Setup

A basic Ubqt setup involves three pieces:
 - One or more Ubqt services (See [list of services](##list-of-services) below)
 - One or more transport servers
 - A client to talk to the server

(See [list of servers and clients](##list-of-servers-and-clients) below)

## Modifying Your ubqt.cfg

For this example, we'll use three services: `ircfs`, `docfs`, and `testfs`. You can find links to them, and other services below. 
Most Ubqt services use a file called ubqt.cfg for their setup. It has a simple format

```
# Example ubqt.cfg

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
# Example2 ubqt.cfg

service=irc server=irc.freenode.net port=6667 ssl=none
	# user details here

service=docs

service=test

```

Add an entry for each service you wish to use to your ubqt.cfg; example entries are found on the main page of each service listed below.

## Running Ubqt

Once your ubqt.cfg is set up, you're mostly good to go. Start up your services:

```
ircfs &
docfs &
testfs &
```

This will create a set of directories, by default in /tmp/ubqt.

```
> ls /tmp/ubqt
/tmp/ubqt/irc
/tmp/ubqt/docs
/tmp/ubqt/test
```

And they're ready to go! Now, these folders are interesting in their own right, but not particularily useful. To interact with these, you need a Ubqt server, and a client that can connect to it. For our example, we'll use 9p-server, and the simple example client that ships with it.

## Server Set Up

`9p-server &` will start the 9p server in the background, listening for connections. The default port for connections is `:564`. (To modify this, see [Using listen_address](using-listen-address.md)

## Client

To connect to your running server is simple.

```
# Using the example client
> example localhost
```

Or if on a remote machine, and your 9p-server is on a machine with the ip address 192.168.1.104

```
# Using example client
> example 192.168.1.104

```

## Commands

As a side note, there are a number of commands you can run from a ubqt client. The whole list depends on the service you are connected to, but the following will always be available:

 - `buffer <buffer>`: change the current viewed buffer
 - `open <buffer>`: open a buffer
   - on IRC, this will connect to a channel. In docfs, it will open the named document at the path sent it. On smsfs, it will start a conversation window with the person at that number
 - `close <buffer>`: closes the buffer, optionally disconnecting any network-based session
 - `quit`: Ends the client session

## Doing Things

In your client, type `/tabs` to see a list of currently opened buffers you can view.

Now, to change to one of those buffers type `buffer <the tab you wish to view>`

This should cause the main document body of the tab you switched to, to be displayed. In the case of a service which constantly outputs new data, such as ircfs or discordfs, you'll see each new line as they come in for that buffer.

If you're connected to an IRC channel, type some text to your friends, and when you're finished, type `/quit` to exit the client.

## List of Services

 - [ircfs](https://github.com/ubqt-systems/ircfs): Connect and chat people on IRC networks
 - [docfs](https://github.com/ubqt-systems/docfs): Read PDF and EPUB files
 - [testfs](https://github.com/ubqt-systems/testfs): Open text files to view and modify text

### Services Planned
 - gamefs: Play retro games!
 - httpfs: Simple web docment viewer
 - discordfs: Connect and chat with people on Discord
 - mailfs: Read, write, and view emails
 - newsfs: RSS aggregator and viewer
 - smsfs: Read, write, and view sms messages
 - undofs: Edit documents with unlimited undo/redo, using the integrated Sam command language 
 - videofs: Search and watch video streams

## List of Servers and Clients

 - [9p server](https://github.com/ubqt-systems/9p-server): Uses 9p from Plan9
 - [9p server example client](https://github.com/ubqt-systems/9p-server/client)
 - [linux-client](https://github.com/ubqt-systems/linux-client): A compile-time plugin based client which allows you to select which server you want to connect to, what type of input you prefer to use, and the graphics drawing library you prefer for how you're using the client.

### Servers Planned

 - html5 server: Access ubqt via a web browser
 - ssh server: Access ubqt through an ssh tunnel

### Clients Planned

 - plan9-client: Simple client for use on the Plan9 operating system
 - plan9-nuklear: A variant of the plan9 client using the Nuklear immediate mode UI library
 - Android/iOS mobile client: Touch-based interface to ubqt
 - Kodi client: linux-client based Kodi plugin
