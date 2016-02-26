# .dependencies
A file to define environment dependencies for applications

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
