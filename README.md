Forked version of github.com/tarm/goserial

This version supports opening a port with RtsOn or DtrOn.

Modifications to this fork
-----------------------
Added the ability to turn on RTS and DTR. This is done by passing in a 
config struct that is setup to support future flags as needed. In particular
the RTS being configured as on was key in supporting the TinyG for the
serial-port-json-server.

(Original Readme Below)

GoSerial
========
A simple go package to allow you to read and write from the
serial port as a stream of bytes.

Details
-------
It aims to have the same API on all platforms, including windows.  As
an added bonus, the windows package does not use cgo, so you can cross
compile for windows from another platform.  Unfortunately goinstall
does not currently let you cross compile so you will have to do it
manually:

    GOOS=windows make clean install

Currently there is very little in the way of configurability.  You can
set the baud rate.  Then you can Read(), Write(), or Close() the
connection.  Read() will block until at least one byte is returned.
Write is the same.  There is currently no exposed way to set the
timeouts, though patches are welcome.

Currently all ports are opened with 8 data bits, 1 stop bit, no
parity, no hardware flow control, and no software flow control.  This
works fine for many real devices and many faux serial devices
including usb-to-serial converters and bluetooth serial ports.

You may Read() and Write() simulantiously on the same connection (from
different goroutines).

Usage
-----
```go
package main

import (
        "github.com/tarm/goserial"
        "log"
)

func main() {
        c := &serial.Config{Name: "COM45", Baud: 115200}
        s, err := serial.OpenPort(c)
        if err != nil {
                log.Fatal(err)
        }
        
        n, err := s.Write([]byte("test"))
        if err != nil {
                log.Fatal(err)
        }
        
        buf := make([]byte, 128)
        n, err = s.Read(buf)
        if err != nil {
                log.Fatal(err)
        }
        log.Print("%q", buf[:n])
}
```

Possible Future Work
-------------------- 
- better tests (loopback etc)
