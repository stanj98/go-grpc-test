# Go gRPC + Protocol Buffers Setup

This project uses **Protocol Buffers (`protoc`)** with Go to generate code for gRPC services.

---

## Prerequisites

Make sure you have the following installed:

* Go (>= 1.20 recommended)
* `protoc` (Protocol Buffers compiler)

### Install `protoc`

#### Ubuntu / Debian

```bash
sudo apt install -y protobuf-compiler
```

#### macOS (Homebrew)

```bash
brew install protobuf
```

#### Verify installation

```bash
protoc --version
```

---

## Install Go Plugins

Install the required Go plugins for protobuf and gRPC:

```bash
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```

---

## Add Go bin to PATH

Ensure Go binaries are in your `PATH`:

```bash
export PATH="$PATH:$(go env GOPATH)/bin"
```

### Make it permanent

#### bash

```bash
echo 'export PATH=$PATH:$(go env GOPATH)/bin' >> ~/.bashrc
source ~/.bashrc
```

#### zsh

```bash
echo 'export PATH=$PATH:$(go env GOPATH)/bin' >> ~/.zshrc
source ~/.zshrc
```

---

## Verify Plugins

```bash
which protoc-gen-go
which protoc-gen-go-grpc
```

Both commands should return paths (e.g., `/home/user/go/bin/...`)

---

## Generate Go Code from `.proto`

Run the following command:

```bash
protoc \
  --go_out=invoicer \
  --go_opt=paths=source_relative \
  --go-grpc_out=invoicer \
  --go-grpc_opt=paths=source_relative \
  invoicer.proto
```

---

## Output

This will generate:

* `invoicer.pb.go` → protobuf message code
* `invoicer_grpc.pb.go` -> gRPC service code

Both files will be created inside the `invoicer/` directory.

---

## Troubleshooting

### Error: `protoc-gen-go: program not found`

**Fix:**

* Ensure plugins are installed
* Ensure `$GOPATH/bin` is in `PATH`

---

### Check Go bin path

```bash
go env GOPATH
```

---

### Temporary workaround

```bash
protoc \
  --plugin=protoc-gen-go=$(go env GOPATH)/bin/protoc-gen-go \
  --go_out=invoicer \
  invoicer.proto
```

---

## Notes

* `--go_opt=paths=source_relative` keeps generated files relative to your `.proto` file
* Always re-run `protoc` after modifying `.proto` files