+++
title = 'Building a CLI in go'
date = 2024-10-22T21:49:08+02:00
draft = false
pin =  true
+++

# Building a CLI in go

Creating a Command Line Interface (CLI) can be a rewarding and fun experience, especially with the powerful combination of Go and Cobra.

Here's a brief overview of how to get started building your own CLI with cobra.

All of this is very simple, since I am a newborn gopher myself. I have not practised too much with go yet but I am planning to expand on my knowledge and keep adding features to my [CLI](https://github.com/sanriodev/goji).

## Getting Started

First, I set up my Go environment and created a new project directory:

```sh
mkdir mycli
cd mycli
go mod init mycli
```

## Installing Cobra

Cobra is a library for creating powerful CLI applications. I installed it using:

```sh
go get -u github.com/spf13/cobra/cobra
```

## Creating the CLI

I used Cobra to initialize my CLI application:

```sh
cobra init --pkg-name mycli
```

This command generated the basic structure of my CLI application, including the main.go file and a cmd directory.

## Adding Commands

Next, I added a simple command. For example, to add a `hello` command, I ran:

```sh
cobra add hello
```

This created a new file `hello.go` in the `cmd` directory. I edited this file to define the behavior of the `hello` command:

```go
package cmd

import (
    "fmt"
    "github.com/spf13/cobra"
)

var helloCmd = &cobra.Command{
    Use:   "hello",
    Short: "Prints a greeting message",
    Run: func(cmd *cobra.Command, args []string) {
        fmt.Println("Hello, world!")
    },
}

func init() {
    rootCmd.AddCommand(helloCmd)
}
```

## Running the CLI

Finally, I built and ran my CLI application:

```sh
go build -o mycli
./mycli hello
```

The output was a simple greeting: `Hello, world!`

## Conclusion

Building a CLI with Go and Cobra was a straightforward and enjoyable process so far. With Cobra's powerful features and Go's performance and beautiful simplicity, I was able to create a (somewhat) useful CLI tool quickly. I look forward to expanding its functionality in the future.

### Checkout Goji

If you want to see my funny little emoji generator CLI tool for yourself visit [Github](https://github.com/sanriodev/goji) or the [go package registry](https://deps.dev/go/github.com%2Fsanriodev%2Fgoji/).
