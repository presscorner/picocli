# picocli - a mighty tiny Command Line Interpreter

A Java command line parsing framework in a single file, so you can include it _in source form_.
This lets users run your application without requiring picocli as an external dependency.

How it works: annotate your class and picocli initializes it from the command line arguments,
converting the input to strongly typed data. Supports any option prefix style,
sub-commands, POSIX-style short groupable options, multi-valued options 
with an exact range of parameters (e.g., `0..*`, `1..2`), and more.

Generates beautiful and easily tailored usage help, using ANSI colors where possible.
Works with Java 5 or higher.

See the [manual](docs/index.adoc) for details.

## Example

Annotate fields with the command line parameter names and description.

```java
import picocli.CommandLine.Option;
import picocli.CommandLine.Parameters;
import java.io.File;

public class Example {
    @Option(names = { "-v", "--verbose" }, description = "Be verbose.")
    private boolean verbose = false;

    @Option(names = { "-h", "--help" }, help = true,
            description = "Displays this help message and quits.")
    private boolean helpRequested = false;

    @Parameters(arity = "1..*", paramLabel = "FILE", description = "File(s) to process.")
    private File[] inputFiles;
    ...
}
```

Then invoke `CommandLine.parse` with the command line parameters and an object you want to initialize.

```java
String[] args = { "-v", "inputFile1", "inputFile2" };
Example app = CommandLine.parse(new Example(), args);

assert !app.helpRequested;
assert  app.verbose;
assert  app.inputFiles != null && app.inputFiles.length == 2;
```

## Usage Help

If the user requested help or if invalid input resulted in a `ParameterException`,
picocli can generate a usage help message. For example:
```java
CommandLine.usage(new Example(), System.out);
```

The generated help message looks like this (colors only rendered when ANSI codes are enabled):

![Usage help message with ANSI colors](docs/ExampleUsageANSI.png?raw=true)

Usage help is highly customizable.
A more elaborate usage help example is shown below:

![Longer help message with ANSI colors](docs/UsageHelpWithStyle.png?raw=true)
