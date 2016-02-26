# .dependencies
A file to define environment dependencies for applications

First let's talk about why. Many different programming communities have ways to
document dependencies required for a program or application to function.

Ruby has the Gemfile, Python has requirements.txt, Node has package.json.

The issue is that there are system-level dependencies that are often omitted
from these dependency documentation systems. For instance it's rare for my Ruby
applications to specify that they require PostgreSQL, Redis and ffmpeg to
function.

There are OS-specific partial solutions like the Brewfile but they're only
partially solving the problem because not everyone uses the same OS, and it's
equally important to specify which version of the software you depend on.

This is why I'm want to try to design a simple file called .dependencies

Two immediate advantages: the file name makes its purpose obvious, and the fact
that it starts with a dot and is followed by a letter that is early in the
alphabet means that it will tend to show up early in a file tree.

Yes, it does make Unix-y assumptions by using the dot-first naming scheme.

I expect that eventually this file will be read by machine more often than by
humans, but I do want to make it as human-friendly as possible considering how
crucial it is.

Now let's talk about the contents. I don't want to make complex pre-requisites
necessary in order for computers to parse this file and understand what it
specifies. This is why a simple key/value format seems like a good idea. Yes, it
does look like YAML, but it doesn't need to be that. Not every machine out there
can parse YAML, and I don't think it should be necessary to install Ruby in
order to parse it otherwise this would be sort of self-defeating, wouldn't it?

The key is an unquoted string, this may come back to haunt me in future
iterations but initially this feels right:

  postgresql: "~> 9.3"

Why a quoted string for the version? Well, because Bundler's format is the best
version specification format I know of. The string allows for complex
modifications on the version number. It allows strict versions specifications
("9.1.3"), open-ended ("> 0.1.0"), ranges ([">= 4.1.0", "< 5.1"]), and even
loose ones ("~> 9.3" meaning equal or greater than 9.3 but less than 9.4).

Finally there's a little bonus, a second value called `file` which hints at 
where each system-level dependency should look for its own dependencies. This 
feels important to me because I don't intend this dependency specification file 
to replace all existing language-specific ones. I think there's a gap in our 
dependency documentations and I just want to fill it, not revolutionize 
everything.

And since everything needs a name, I'm calling it Dependent. I'm sure you'll 
appreciate the aliteration: "The Dependent dependencies file". Rolls off the 
tongue, doesn't it? :-)

## Example

In a file named `.dependencies` at the root of your project:

```ruby
ruby: "~> 2.3", file: "Gemfile"
postgres: "~> 9.3"
python: "~> 2.7", file: "requirements.txt"
node: "~> 5.5", file: "package.json"
```

## Details

Take the line `ruby: "~> 2.3", file: "Gemfile"`. Let's break it down.

1. `ruby` is the name of the system-level software being dependend on by your program
2. `"~> 2.3"` defines the version constraint for the `ruby` dependency, `~>` means "loose" which is equivalent to saying "I want any version including 9.3 and above but below 9.4".
3. `file` is an optional key that allows you to specify where the system-level dependency can find its own dependencies, and in Ruby programs usually document their dependencies using a Bundler `Gemfile`.
