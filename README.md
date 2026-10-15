# Telemetry Proto

This repository contains shared Protocol Buffer (`.proto`) definitions for a telemetry collection system. This contract is used by both the client (Sensor Node) and the server (Telemetry Sink) to ensure strict typing during gRPC communication.

## 🛠 Environment requirements

To generate Go code from `.proto` files, you will need to install the Protocol Buffers compiler (`protoc`) and the appropriate plugins for Go.

### 1. Installing the Protocol Buffers compiler (protoc)

You need the `protoc` compiler itself. Install it according to your operating system:

**Ubuntu / Debian:**
```bash
sudo apt update
sudo apt install -y protobuf-compiler
```

*Verify the installation was successful using the command:*
```bash
protoc --version
```

### 2. Installing plugins for Go

After installing the compiler, you need to install plugins that will enable `protoc` to generate code specifically for Go and gRPC.

Execute the following commands:
```bash
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```

**Important:** Ensure that the `GOPATH/bin` directory is added to your system's `PATH` environment variable; otherwise, `protoc` will not be able to find the installed plugins.
Add this line to your `~/.bashrc` or `~/.zshrc`:
```bash
export PATH="$PATH:$(go env GOPATH)/bin"
```

---

## 🚀 Compiling telemetry.proto

If you have made changes to the contract (for example, by adding new fields to `api/proto/v1/telemetry.proto`), you need to regenerate the Go code.

While in the root of the `telemetry-proto` directory, run the following command:

```bash
protoc --go_out=. \
       --go_opt=paths=source_relative \
       --go-grpc_out=. \
       --go-grpc_opt=paths=source_relative \
       api/proto/v1/telemetry.proto
```

### What does this command do?
* `--go_out=.` — generates Go data structures (Protobuf messages).
* `--go-grpc_out=.` — generates client and server gRPC interfaces.
* `paths=source_relative` — instructs the compiler to place the generated `.pb.go` and `_grpc.pb.go` files in the same directory as the source `.proto` file (in the `api/proto/v1/` folder).

Upon successful execution, you will see the updated files:
* `api/proto/v1/telemetry.pb.go`
* `api/proto/v1/telemetry_grpc.pb.go`

Be sure to commit these generated files along with your `.proto` file so that other microservices can use them.
