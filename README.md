# ma-pipe - multiaddr pipes

> multiaddr powered pipes

This is a simple program, much like netcat or telnet, that sets up pipes between multiaddrs.

## Install

For now, use `go get` to install it:

```
go get -u github.com/multiformats/ma-pipe/ma-pipe
ma-pipe --version # should work
```

Please file an issue if there is a problem with the install.

## Usage

`ma-pipe` sets up simple pipes based on multiaddrs. It has four modes:

- `listen` will listen on 2 given multiaddrs, accept 1 conn each, and pipe the connection together
- `dial` will dial to 2 given multiaddrs, and pipe the connection together
- `fwd` will listen on 1 multiaddr, accept 1 conn, then dial the other given multiaddr
- `proxy` will listen on 1 multiaddr, accept 1 conn, read a multiaddr from the conn, and dial it

Notes:

- `ma-pipe` supports "zero" listen multiaddrs (eg `ma-pipe proxy /ip4/0.0.0.0/tcp/0`)
- `ma-pipe` supports the `/unix/stdio` multiaddr (eg `ma-pipe fwd /unix/stdio /ip4/127.0.0.1/tcp/1234`)

### CLI Usage Text

```sh
> ma-pipe --help
USAGE
	ma-pipe <mode> <multiaddrs>...

	ma-pipe listen <listen-multiaddr1> <listen-multiaddr2>
	ma-pipe dial <dial-multiaddr1> <dial-multiaddr2>
	ma-pipe fwd <listen-multiaddr> <dial-multiaddr>
	ma-pipe proxy <listen-multiaddr>

OPTIONS
	-h, --help          display this help message
	-v, --version       display the version of the program
	-t, --trace <dir>   save a trace of the connection to <dir>

EXAMPLES
	# listen on two multiaddrs, accept 1 conn each, and pipe them
	ma-pipe listen /ip4/127.0.0.1/tcp/1234 /ip4/127.0.0.1/tcp/1234

	# dial to both multiaddrs, and pipe them
	ma-pipe dial /ip4/127.0.0.1/tcp/1234 /ip4/127.0.0.1/tcp/1234

	# listen on one multiaddr, accept 1 conn, dial to the other, and pipe them
	ma-pipe fwd /ip4/127.0.0.1/tcp/1234 /ip4/127.0.0.1/tcp/1234

	# listen on one multiaddr, accept 1 conn.
	# read the first line, parse a multiaddr, dial that multiaddr, and pipe them
	ma-pipe proxy /ip4/127.0.0.1/tcp/1234

	# ma-pipe supports "zero" listen multiaddrs
	ma-pipe proxy /ip4/0.0.0.0/tcp/0

	# ma-pipe supports the /unix/stdio multiaddr
	ma-pipe fwd /unix/stdio /ip4/127.0.0.1/tcp/1234
```

### Traces

The `-t, --trace` option allows the user to specify a directory to capture a trace of the connection. Three files will be written:

- `<trace-dir>/ma-pipe-trace-<date>-<pid>-a2b` for one side of the (duplex) connection.
- `<trace-dir>/ma-pipe-trace-<date>-<pid>-b2a` for the other side of the (duplex) connection.
- `<trace-dir>/ma-pipe-trace-<date>-<pid>-ctl` for control messages.

```
> tree mytraces
mytraces
├── ma-pipe-trace-2016-09-12-03:35:31Z-14088-a2b
├── ma-pipe-trace-2016-09-12-03:35:31Z-14088-b2a
└── ma-pipe-trace-2016-09-12-03:35:31Z-14088-ctl
```

## License

MIT
