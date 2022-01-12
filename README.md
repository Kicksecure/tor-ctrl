# tor-ctrl

Raw use of tor's controller. Inspired on [stem](stem.torproject.org) and fork of [tor-ctrl](https://github.com/adrelanos/tor-ctrl/blob/master/usr/bin/tor-ctrl).

## Requirements

At least one of each item is necessary.

* **nc**/**socat**/**telnet**
* **xxd**/**hexdump**/**od**

## Features

**ControlPort**/**ControlSocket**: the following socket types are accepted to connect to the controller:
* unix domain socket, specified as `[unix:]path`
* tcp socket, specified as `[addr:]port`, if empty, will try port 127.0.0.1:9051.

**Authentication methods**:
* ~~SAFECOOKIE~~ (on the work, help wanted)
* COOKIE
* HASHEDPASSWORD

**CookieAuthFile**: discover it by sending PROTOCOLINFO, so no need to specify the file.

## Installation

### Control method

This will be the socket that allows those connections to control the Tor process. Choose between `ControlPort` and `ControlSocket` (setting both means either control can be used).

Note: the options must be set inside your torrc.

Port:
```sh
ControlPort 9051
```

Socket:
```sh
ControlSocket /var/run/tor/control
```

### Authentication method

This is will be the method you will authenticate to the controller. Choose between `CookieAuthentication` and `HashedControlPassword` (Setting both authentication methods means either method is sufficient to authenticate to Tor)

Note: the options must be set inside your torrc.

Cookie:
```sh
CookieAuthentication 1
```
or
Password (change `yourpassword`, but maintain it double quoted)
```
printf '%s\n' "HashedControlPassword $(tor --hash-password "yourpassword")"
```

### Apply the changes

If you have made any changes to the tor run commands file (torrc), you will need to HUP tor to apply the new configuration.

```sh
pkill -sighup tor
```

## Usage

Install as root:
```sh
./configure.sh install
```

See usage:
```sh
tor-ctrl -h
```

Get your tor user:
```sh
tor-ctrl GETCONF User"
```

Get your circuits (raw):
```sh
tor-ctrl GETINFO circuit-status
```

That is not very clean to read, too much information, so lets organize it.
```sh
./examples/circuit.sh
```
Much better now huh?

And if we could see the streams and to which circuit they are attached to and what is their target host?
```sh
./examples/stream.sh
```
Now, from another terminal, connect via tor to where you wish:
```sh
curl -x socks5h://127.0.0.1:9050 github.com
```
Now, go back to the script and press enter to print out the events received.

## Useful links

* [tor manual](https://2019.www.torproject.org/docs/tor-manual-dev.html.en#cookieauthentication)
* [control-spec](https://gitweb.torproject.org/torspec.git/tree/control-spec.txt#n1637)

## History

**tor-ctrl** was originally named **tor-ctrl** and created by Stefan Behte, later developed by Patrick Schleizer and later continued by nyxnor.

## License

tor-ctrl is GPLv3, the rest is MIT.
