---
title: "Writing Services"
---

# Writing Services

[fslib](https://github.com/altid/fslib) and [cleanmark](https://github.com/altid/cleanmark) were designed to trivialize writing conforment services.

 - fslib provides a control file for issuing messages from a client to a service, as well as a file to provide input that is directed at a given `buffer` of a service. A buffer would include something like an IRC channel, sms message thread, etc.
 - cleanmark provides facilities to escape markdown elements from raw text, convert html, as well as providing a mechanism to do the inverse through tokenization.

> The libraries mentioned are all programmed, and meant for use with Golang.
> Refer to [testfs](https://github.com/altid/testfs) for a full, simple implementation

## Controller

fslib exposes a type called a Controller. Due to how Golang interfaces work, any type can double as a controller, granted that it has three specific methods with the same definition (so, the same types of arguments to, and returning the same type of argments from, the ones listed below)

```
Open(c *Control, filename string) error
Close(c *Control, filename string) error
Link(c *Control, from filename string) error
Default(c *Control, cmd, from, msg string) error
```

For our purposes, it suffices to know that the above messages will be called whenever a client writes `open <foo>`, `close <foo>`, `link <foo>`, or `somecmd <from buffer> <foo>`; and you'll be able to respond to it however you'd like. Canonically, `open` should always try to create a buffer for the named resource; such as joining a channel on a chat server, opening a text file for viewing, opening a particular web page; and close should do the inverse, removing the buffer itself from the eventual directories you create. Link should take the current viewed buffer, and replace it outright with the resource located at foo.

Let's try one

```go
package main

import (
	"github.com/altid/fslib"
)

type foo struct {
	// some items here of interest
}

func main() {
	// error handling elided
	c, _ := fslib.CreateCtrlFile(foo, "none", "/tmp/altid", "foo", "document")
	c.Listen()
}

func (f *foo) Open(c *fslib.Control, filename string) error {
	// fslib also exposes helper libs
	// here we create a buffer, giving the main file name we wish to use throughout
	// this must match what we include for CreateCtrlFile
	return c.CreateBuffer(filename, "document")
}

func (f *foo) Close(c *fslib.Control, filename string) error {
	return c.DeleteBuffer(filename, "document")
}

func (f *foo) Link(c *fslib.Control, from, filename string) error {
	c.DeleteBufer(from, "document")
	f.Open(c, filename)
}

func (f *foo) Default(c *fslib.Control, cmd, from, msg string) error {
	switch cmd {
	case "foo":
		// [...]
	case "bar":
		// [...]
	}
	return nil
}
```

Generally, you'll want to do something more than just create an empty file, but this serves as a decent skeleton for further work.

## Cleaning Text

Consider something like the following

```go
func (f *foo) Open(c *fslib.Control, filename string) error {
	// errors elided
	c.CreateBuffer(path.Join("/tmp/altid", filename), "document")
	// MainWriter will write content to the correct "document"
	mainWriter := c.MainWriter(filename, "document")
	data, _ := ioutil.ReadFile(filename)
	mainWriter.Write(data)
	return err
}

```

All content will be written and presented to the client unmodified. To avoid unwanted tokens being misinterpreted as markdown, we need to escape all the text.

```go
func (f *foo) Open(c *fslib.Control, filename string) error {
	c.CreateBuffer(path.Join("/tmp/altid", filename), "document")
	mainWriter := c.MainWriter(filename, "document")
	data, _ := ioutil.ReadFile(filename)
	// cleanmark can accept a writer directly from fslib
	cleaner := cleanmark.NewCleaner(mainWriter)
	defer cleaner.Close()
	_, err := cleaner.WriteEscaped(data)
	return err	
}
```

## Further Reading

 - [fslib godoc](https://godoc.org/github.com/altid/fslib)
 - [cleanmark godoc](https://godoc.org/github.com/altid/cleanmark)
