![Command Cougar](https://raw.githubusercontent.com/surfandneptune/CommandCougar/master/Assets/logo-padded.png)

An elegant pure Swift library for building command line applications.


## Features

- [x] Light weight, simple, composed with [pure value types.](https://developer.apple.com/videos/play/wwdc2015/414/)
- [x] Auto generated help menus for main command and sub-commands.
- [x] Help menus are format is based on [swift package manager](https://github.com/apple/swift-package-manager)
- [x] Supports multi command callbacks.
- [x] Swift 3 compatibility
- [x] Zero dependency
- [x] Supports Linux and `swift build`

## Requirements

- Mac OS X 10.10+ / Ubuntu 14.10
- Xcode 8

---

## Usage
  CommandCougar supports a main command as well as subcommands.  This is much like
  the swift package manager interface.  

#### Consider this command

``` shell
swift package -v update --repin
```

`swift` is the main command.  

`package` is a subcommand of the `swift` command with `-v` as an option.

`update` is a subcommand of the `package` command with `--repin` as an option.


### Simple Example
This example parses `swift package -v update --repin` and performs
an `echo` callback each command

``` swift
func echo(evaluation: Command.Evaluation) throws {
  print(
    "\(evaluation.name) evaluated with " +
    "options: \(evaluation.options) " +
    "and parameters \(evaluation.parameters)"
    )
}

let swiftCommand =
Command.Description(
    name: "swift",
    overview: "Swift Program",
    callback: echo,
    options: [],
    subCommands: [
        Command.Description(
            name: "package",
            overview: "Perform operations on Swift packages",
            callback: echo,
            options: [
                Option.Description(
                    flag: .both(short: "v", long: "verbose"),
                    overview: "Increase verbosity of informational output"),
                Option.Description(
                    flag: .long("enable-prefetching"),
                    overview: "Increase verbosity of informational output")
            ],
            subCommands: [
                Command.Description(
                    name: "update",
                    overview: "Update package dependencies",
                    callback: echo,
                    options: [
                        Option.Description(
                            flag: .long("repin"),
                            overview: "Update without applying pins and repin the updated versions.")
                    ],
                    subCommands: [])
            ])
    ])


do {
  // args normally is CommandLine.arguments
    let args = ["swift", "package", "-v", "update", "--repin"]
    let evaluation: Command.Evaluation = try swiftCommand.evaluate(arguments: args)
    try evaluation.performCallbacks()
} catch {
    print(error)
}

// Output
// swift evaluated with  options: []  and parameters []
// package evaluated with  options: [-v]  and parameters []
// update evaluated with  options: [--repin]  and parameters []
```

### Accessing the values of the `Command.Evaluation`
To directly access the values of the returned `Command.Evaluation`
``` swift
evaluation["package"]?.name  // results in "package"

evaluation["package"]?.options["v"] // results in Option.Evaluation

evaluation["package"]?.options["v"]?.flag.shortName // results in "v"

evaluation["package"]?.options["enable-prefetching"] // results in nil

evaluation["package update"]?.options["repin"]?.flag.longName // results in "repin"
```

### Creating a Command.Description

The `Command.Description` struct is used to represent how this command should be
parsed.

Here is a `Command.Description` that will evaluate the above command.
``` swift
let swiftCommand =
Command.Description(
    name: "swift",
    overview: "Swift Program",
    callback: swiftCallback,
    options: [],
    subCommands: [
      Command.Description(
        name: "package",
        overview: "Perform operations on Swift packages",
        callback: packageCallback,
        options: [
          Option.Description(
            flag: .both(short: "v", long: "verbose"),
            overview: "Increase verbosity of informational output"),
            Option.Description(
              flag: .both(long: "enable-prefetching"),
              overview: "Increase verbosity of informational output")
        ],
        subCommands: [
          Command.Description(
            name: "update",
            overview: "Update package dependencies",
            callback: updateCallback,
            options: [
              Option.Description(
                flag: .long("repin"),
                overview: "Update without applying pins and repin the updated versions.")
            ],
            subCommands: [])
        ])
    ])

```

### Evaluating the `Command.Description` to create a `Command.Evaluation`

``` swift
let args = CommandLine.arguments
let evaluation: Command.Evaluation = try swiftCommand.evaluate(arguments: args)
```

### Performing callbacks on a `Command.Evaluation`

```swift
try evaluation.performCallbacks()
```
This will perform three callbacks:

`swiftCallback`: No options and no commands passed

`packageCallback`: `-v` as an option

`updateCallback`: `--repin` as an option


## Communication

- If you **found a bug**, open an issue.
- If you **have a feature request**, open an issue.
- If you **want to contribute**, open an issue or submit a pull request.

## Installation



### CocoaPods



### [Swift Package Manager](https://github.com/apple/swift-package-manager)

```swift
dependencies: [
    .Package(url: "https://github.com/surfandneptune/CommandCougar.git", majorVersion: 1)
]
```

### Carthage

[Carthage](https://github.com/Carthage/Carthage) is a decentralized dependency manager that automates the process of adding frameworks to your Cocoa application.

You can install Carthage with [Homebrew](http://brew.sh/) using the following command:

``` bash
$ brew update
$ brew install carthage
```

To integrate CommandCougar into your Xcode project using Carthage, specify it in your `Cartfile`:

``` ogdl

```

---

## FAQ



## Credits



## Changelog



## License

CommandCougar is released under the MIT license. See LICENSE for details.