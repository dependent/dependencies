# .dependencies
A file to define environment dependencies for applications

## Purpose
Different programming communities have ways to document dependencies 
required for a program or application to function.

Ruby has the [`Gemfile`](http://bundler.io/gemfile.html), 
Python has [`requirements.txt`](https://pip.readthedocs.org/en/1.1/requirements.html), 
and Node has [`package.json`](https://docs.npmjs.com/files/package.json).

The `.dependencies` file expands on those good ideas in order to provide 
system-level dependency documentation.

## Example

In a file named `.dependencies` at the root of your project:

```ruby
ruby: "~> 2.3", file: "Gemfile"
postgres: "~> 9.3"
python: "~> 2.7", file: "requirements.txt"
node: "~> 5.5", file: "package.json"
```

Let's break it down, by focusing on first line:

1. `ruby` is the name of the system-level software being dependend on by your program
2. `"~> 2.3"` defines the version constraint for the `ruby` dependency, `~>` means "loose" which is equivalent to saying "I want any version including 9.3 and above but below 9.4".
3. `file` is an optional key that allows you to specify where the system-level dependency can find its own dependencies, and in Ruby programs usually document their dependencies using a Bundler `Gemfile`.

## Why document dependencies in a file?

The issue is that there are system-level dependencies that are often omitted
from these dependency documentation systems. For instance it's rare for my Ruby
applications to specify that they require PostgreSQL, Redis and ffmpeg in 
order to function.

There are OS-specific solutions like the 
[Brewfile](https://robots.thoughtbot.com/brewfile-a-gemfile-but-for-homebrew) 
but they're only partially solving the problem because not everyone uses the 
same operating system (OS X in this case), and it's equally important to specify 
which version of the software you depend on.

## Benefits of `.dependencies`

### Obvious
The file name makes its purpose obvious. It starts with a `.` and will 
show up early in the file tree because the letter "d" is early in the 
alphabet.

### Clarity
Now let's talk about the contents of the `.dependencies` file. 

Eventually this file will be read by machine more often than by humans 
but considering how crucial it is to developers, it needs to be as 
human-friendly as possible.

Parsing the file needs to be easy for both humans and machines. This is why
it uses a simple key/value format. It does *look* like YAML, but it doesn't need 
to be. Not every machine out there can parse YAML. 

```ruby
  postgresql: "~> 9.3"
```

### Versions
Why a quoted string for the version? Well, because Bundler's format is the best
version specification format I know of. The string allows for complex
modifications on the version number. It allows strict versions specifications
(`"9.1.3"`), open-ended (`"> 0.1.0"`), ranges (`[">= 4.1.0", "< 5.1"]`), and even
loose ones (`"~> 9.3"` meaning equal or greater than 9.3 but less than 9.4).

### Integrations
Finally there's a little bonus, a second value called `file` which hints at 
where each system-level dependency should look for its own dependencies. This 
feels important to me because I don't intend this dependency specification file 
to replace all existing language-specific ones. I think there's a gap in our 
dependency documentations and I just want to fill it, not revolutionize 
everything.
