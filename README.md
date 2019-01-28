# Lab: ANTLR Lexer

In this lab, we will be experimenting with Antlr's lexer grammars.
You will be performing the following:

- Define token types and their patterns in a lexer grammar.
- Implement a _driver_ application to perform the lexical analysis.
- Use the Antlr TestRig tool to perform the lexical analysis.
- Use gradle to build your application.

---

Consider the following Java program:

`Sample.java`

```java
import java.util.*;

public class App {
  public static void main(String [] args) {
    List<String> arglist = Arrays.asList(args);
    int lastIndex = argslist.size() - 1;
    Collections.reverse(arglist);
    for(String s : arglist) {
        System.out.println(s);
    }
  }

  @Override
  public String toString() {
    return "Ding: this is the main app." + 3.1415;
  }
}
```

# Define the token types

Let's build an ANTLR grammar.  Call it `JLexer.g4`

Your file should be like:

`JLexer.g4`
> ```
> lexer grammar JLexer;
>
> ...
> ```

Your tokenss need to include (but not limited to):

- various keywords, e.g. import, public, static, void
- primitive types, e.g. int, float
- identifier
- String literals
- Number literals.
- Puntunations, e.g. `;`, `[`, `]`, ...
- Whitespaces which should also include newlines

For each token, define the lexer rules in the form of:

```
<RuleName> : <RulePattern> ( <optional Action> ) ;
```

**Note**

1. Recall that the lexer rule names must start with a uppercase letter.
2. Ignore whitespaces.

# Compile the lexer

Make sure you can:

1. Generate `JLexer.java`
2. Compile to `JLexer.class`

# TestRig

Apply the produced `JLexer.class` on `Sample.java` using the built-in Antlr
TestRig.

Recall:

1. The test rig is an executable class `org.antlr.v4.gui.TestRig`
   that comes `antlr-4.7.2-complete.jar`.
2. Make sure you include *both* the antlr jar file and the
   current directory `.` in the classpath when running the the test rig.

Using the test rig, make sure that your lexer is working
without any errors, namely, the entire `Sample.java` source
is properly recognized by the lexer grammar.

# Write an app

Now, write an app program to perform lexical analysis programmatically _without_
the need of the test rig.

Write a program `Main.java`:

```
import org.antlr.v4.runtime.*;

public class Main {
  public static void main(String[] args) {
    String sourcefile = args[0];

    // ------------------------------------------
    // complete the rest of the implementation
    // ------------------------------------------
  }
}
```

Remember that you follow these steps in generating the tokens:

1. Construct a CharStream instance using factory methods of
   `CharStreams.from__`.

2. Construct the lexer instance.

3. Construct a CommonTokenStream instance, and `.fill()` the stream
with tokens.

4. You can then get a `List<Token>` from the commontoken stream.

Refer to the lecture notes, and the online documentation:
https://www.antlr.org/api/Java/index.html

# Using Gradle

In this section, we will modify the directory structure so that we can use
Gradle to manage the compilation and execution process.

## Build the _correct_ directory structure

Gradle expects the files to be in the proper places.
The two source files: `JLexer.g4` and `Main.java` need to be placed in
the right places.  We also need a build.gradle file.  So, you need to arrange to
have the following directory structure.

> ```
> ./build.gradle
> ./src
> ├── main
> │   ├── antlr
> │   │   └── JLexer.g4
> │   └── java
> │       └── Main.java
> └── test
>     └── java
> ```

The `build.gradle` file needs to specify the following:

- Dependency on the antlr library.
- Load antlr plugin
- The main application and its command line argument

It should look like this:

`build.gradle`

> ```
> plugins {
>     id 'antlr'
>     id 'application'
> }
> 
> repositories {
>     mavenCentral()
> }
> 
> dependencies {
>     antlr 'org.antlr:antlr4:4.7.2'
>     compile 'junit:junit:4.12'
>     testCompile 'junit:junit:4.12'
> 
> }
> 
> application {
>     mainClassName = 'Main'
> }
> 
> run {
>     args = ['Sample.java']
> }
> 
> apply plugin: 'java'
> ```

Now try:

```
$ gradle bulid
$ gradle run
```

It should work _even without_ the antlr jar file present.

There, you no longer need to worry about classpath or reproducibility
of your project.
