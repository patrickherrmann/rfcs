- Start Date: 2014-11-17
- RFC PR: (leave this empty)
- Rust Issue: (leave this empty)

# Summary

Introduce a `function!()` macro that expands to an `Option<&'static str>` of the function
name it's used in.

# Motivation

For error reporting cases we already have `file!`, `line!`, `col!` and `module_path!`
but there is no way yet to figure out in which function an error was reported.  Python's
tracebacks are well received and for many people the function name information is much
more important than the line number.

# Detailed design

The behavior of the new `function!` macro should be as such:

* when used within a proper function `fn` (or trait method) it expands to
  `Some(name)` where `name` is the name of the function.  It is always the
  local name of the function which means even if a function is contained in
  another function, it's just the the actual name.
* when used inside a local block in a function (proc, closure or anything else) it
  expands to the name of the innermost proper function.
* if used anywhere else it expands to `None`.

# Drawbacks

Maybe `function!` is a bit generic of a name.  Alternative names could be considered.

# Alternatives

As an alternative name, `function_name!` could be used.
