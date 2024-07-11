## Table of Contents

- [PC](#pc)


|   |   |
|---|---|
|99ADAF8BA950EAE6D11BEE7D872CFE09|"kanderson"|


# PC
port 50051 (gRPC) open.
RPC - Remote Procedure Call
```bash
./grpcurl [flags] [address] [list|describe] [symbol]

The 'address' is only optional when used with 'list' or 'describe' and a
protoset or proto flag is provided.

If 'list' is indicated, the symbol (if present) should be a fully-qualified
service name. If present, all methods of that service are listed. If not
present, all exposed services are listed, or all services defined in protosets.

If 'describe' is indicated, the descriptor for the given symbol is shown. The
symbol should be a fully-qualified service, enum, or message name. If no symbol
is given then the descriptors for all exposed or known services are shown.

If neither verb is present, the symbol must be a fully-qualified method name in
'service/method' or 'service.method' format. In this case, the request body will
be used to invoke the named method. If no body is given but one is required
(i.e. the method is unary or server-streaming), an empty instance of the
method's request type will be sent.

The address will typically be in the form "host:port" where host can be an IP
address or a hostname and port is a numeric port or service name. If an IPv6
address is given, it must be surrounded by brackets, like "[2001:db8::1]". For
Unix variants, if a -unix=true flag is present, then the address must be the
path to the domain socket.

Available flags:
  -H value
        Additional headers in 'name: value' format. May specify more than one
        via multiple flags. These headers will also be included in reflection
        requests to a server.
  -allow-unknown-fields
        When true, the request contents, if 'json' format is used, allows
        unknown fields to be present. They will be ignored when parsing
        the request.
  -authority string
        The authoritative name of the remote server. This value is passed as the
        value of the ":authority" pseudo-header in the HTTP/2 protocol. When TLS
        is used, this will also be used as the server name when verifying the
        server's certificate. It defaults to the address that is provided in the
        positional arguments.
  -cacert string
        File containing trusted root certificates for verifying the server.
        Ignored if -insecure is specified.
  -cert string
        File containing client certificate (public key), to present to the
        server. Not valid with -plaintext option. Must also provide -key option.
  -connect-timeout float
        The maximum time, in seconds, to wait for connection to be established.
        Defaults to 10 seconds.
  -d string
        Data for request contents. If the value is '@' then the request contents
        are read from stdin. For calls that accept a stream of requests, the
        contents should include all such request messages concatenated together
        (possibly delimited; see -format).
  -emit-defaults
        Emit default values for JSON-encoded responses.
  -expand-headers
        If set, headers may use '${NAME}' syntax to reference environment
        variables. These will be expanded to the actual environment variable
        value before sending to the server. For example, if there is an
        environment variable defined like FOO=bar, then a header of
        'key: ${FOO}' would expand to 'key: bar'. This applies to -H,
        -rpc-header, and -reflect-header options. No other expansion/escaping is
        performed. This can be used to supply credentials/secrets without having
        to put them in command-line arguments.
  -format string
        The format of request data. The allowed values are 'json' or 'text'. For
        'json', the input data must be in JSON format. Multiple request values
        may be concatenated (messages with a JSON representation other than
        object must be separated by whitespace, such as a newline). For 'text',
        the input data must be in the protobuf text format, in which case
        multiple request values must be separated by the "record separator"
        ASCII character: 0x1E. The stream should not end in a record separator.
        If it does, it will be interpreted as a final, blank message after the
        separator. (default "json")
  -format-error
        When a non-zero status is returned, format the response using the
        value set by the -format flag .
  -help
        Print usage instructions and exit.
  -import-path value
        The path to a directory from which proto sources can be imported, for
        use with -proto flags. Multiple import paths can be configured by
        specifying multiple -import-path flags. Paths will be searched in the
        order given. If no import paths are given, all files (including all
        imports) must be provided as -proto flags, and grpcurl will attempt to
        resolve all import statements from the set of file names given.
  -insecure
        Skip server certificate and domain verification. (NOT SECURE!) Not
        valid with -plaintext option.
  -keepalive-time float
        If present, the maximum idle time in seconds, after which a keepalive
        probe is sent. If the connection remains idle and no keepalive response
        is received for this same period then the connection is closed and the
        operation fails.
  -key string
        File containing client private key, to present to the server. Not valid
        with -plaintext option. Must also provide -cert option.
  -max-msg-sz int
        The maximum encoded size of a response message, in bytes, that grpcurl
        will accept. If not specified, defaults to 4,194,304 (4 megabytes).
  -max-time float
        The maximum total time the operation can take, in seconds. This is
        useful for preventing batch jobs that use grpcurl from hanging due to
        slow or bad network links or due to incorrect stream method usage.
  -msg-template
        When describing messages, show a template of input data.
  -plaintext
        Use plain-text HTTP/2 when connecting to server (no TLS).
  -proto value
        The name of a proto source file. Source files given will be used to
        determine the RPC schema instead of querying for it from the remote
        server via the gRPC reflection API. When set: the 'list' action lists
        the services found in the given files and their imports (vs. those
        exposed by the remote server), and the 'describe' action describes
        symbols found in the given files. May specify more than one via multiple
        -proto flags. Imports will be resolved using the given -import-path
        flags. Multiple proto files can be specified by specifying multiple
        -proto flags. It is an error to use both -protoset and -proto flags.
  -protoset value
        The name of a file containing an encoded FileDescriptorSet. This file's
        contents will be used to determine the RPC schema instead of querying
        for it from the remote server via the gRPC reflection API. When set: the
        'list' action lists the services found in the given descriptors (vs.
        those exposed by the remote server), and the 'describe' action describes
        symbols found in the given descriptors. May specify more than one via
        multiple -protoset flags. It is an error to use both -protoset and
        -proto flags.
  -protoset-out string
        The name of a file to be written that will contain a FileDescriptorSet
        proto. With the list and describe verbs, the listed or described
        elements and their transitive dependencies will be written to the named
        file if this option is given. When invoking an RPC and this option is
        given, the method being invoked and its transitive dependencies will be
        included in the output file.
  -reflect-header value
        Additional reflection headers in 'name: value' format. May specify more
        than one via multiple flags. These headers will *only* be used during
        reflection requests and will be excluded when invoking the requested RPC
        method.
  -rpc-header value
        Additional RPC headers in 'name: value' format. May specify more than
        one via multiple flags. These headers will *only* be used when invoking
        the requested RPC method. They are excluded from reflection requests.
  -servername string
        Override server name when validating TLS certificate. This flag is
        ignored if -plaintext or -insecure is used.
        NOTE: Prefer -authority. This flag may be removed in the future. It is
        an error to use both -authority and -servername (though this will be
        permitted if they are both set to the same value, to increase backwards
        compatibility with earlier releases that allowed both to be set).
  -unix
        Indicates that the server address is the path to a Unix domain socket.
  -use-reflection
        When true, server reflection will be used to determine the RPC schema.
        Defaults to true unless a -proto or -protoset option is provided. If
        -use-reflection is used in combination with a -proto or -protoset flag,
        the provided descriptor sources will be used in addition to server
        reflection to resolve messages and extensions.
  -user-agent string
        If set, the specified value will be added to the User-Agent header set
        by the grpc-go library.
  -v    Enable verbose output.
  -version
        Print version.
  -vv
        Enable very verbose output.





```

```bash
$ ./grpcurl -plaintext 10.129.41.175:50051 list
SimpleApp
grpc.reflection.v1alpha.ServerReflection
$
```

With this we can see that it is running the `SimpleApp` service. We can also try to list the possible methods to invoke with the `list` command followed by the service as follows:

```bash
./grpcurl -plaintext 10.129.41.175:50051 list SimpleApp
SimpleApp.LoginUser
SimpleApp.RegisterUser
SimpleApp.getInfo
$
```


This tells us that we can invoke the `getInfo`, `RegisterUser` and the `LoginUser` within the service.

By invoking the `getInfo` method we are presented with the following response:

```bash
$ ./grpcurl -plaintext 10.129.41.175:50051 SimpleApp.getInfo
{
  "message": "Authorization Error.Missing 'token' header"
}
$
```

As we can see we are required to have a `token` to be able to interact with it.