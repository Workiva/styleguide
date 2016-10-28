Workiva Go Style Guide
======================

1. Follow

- [Effective Go](https://golang.org/doc/effective_go.html)
- [A Note About Git Commit Messages](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)
- [Go Best Practices](https://peter.bourgon.org/go-best-practices-2016/)
- [Receivers In Go](https://blog.heroku.com/neither-self-nor-this-receivers-in-go)
- [Functional Options for Friendly APIs](http://dave.cheney.net/2014/10/17/functional-options-for-friendly-apis)

2. Make sure your editor automatically runs gofmt on your source code.
3. Prefer wrapping lines to 80 characters, but avoid breaking string literals in error messages. **Justification**: If you break string literals, you break grep.
4. Prefer the %q and %+v directives over the %v directive in errors and logging messages. **Justification**: %v can result in ambiguous error messages when strings are used because the value is not encapsulated.
5. Don’t implement the magic String() method. **Justification**: If you accidentally use Printf the wrong way, you’ll end up in an infinite recursion loop and crash the server. This is hard to catch in testing.
6.  Mocks constructed with [testify](https://github.com/stretchr/testify) must follow these rules:

- They must be publicly available. **Justification**: A private mock is not reusable and leads to dangerous copy-pasting.
- They must exist in the same repository as the interface they mock. **Justification**: When the interface changes, the developer will also update the mock as part of the same PR. This prevents extraneous, annoying, PRs and keeps breakage to a minimum.
- Prefer keeping the mocks in a separate `/mock` package that is imported by tests only. **Justification**: The mocks should not be included in the production binary.

7. If your test does ANYTHING on the network that is not directly set up by the test itself (e.g. connecting to a remote SQL instance, pinging www.google.com, or just expecting to find Redis running on your localhost), you must think hard on the question "do I really need this?". If the answer is definitively yes, add the following block to the beginning of the test. **Justification**: Even though you’ve broken "go test ./...", other devs need "go test -short ./..."

```
if testing.Short() {
	t.Skip()
}
```

8. Don’t use [dot imports](http://stackoverflow.com/a/6478990). **Justification**: They pollute the local namespace and confuse language tooling.
9. The "go test" command provides a "-race" flag that automatically detects data races. Ideally, your continuous integration server should run your tests with "-race". **Justification**: Catching data races is hard. If you enforce a race detector, every build will increase your chances of finding synchronization bugs in your code.
10. Decide on a convention to use to ensure thread safety in a multithreaded environment.  For example, to prevent deadlocks you may make a rule that all exported methods/functions are responsible for obtaining/releasing mutexes and all non-exported functions are to remain lock-free.  Deciding on a convention like this makes it easier to prevent deadlocks.
11. Only import the "flag" package from main.go or a dedicated configuration package. Similarly, only import your dedicated config package from the main package or another configuration package. **Justification**: It's wise to keep the flag package imports contained, since obscure compilation issues can be caused by declaring global flags or calling Parse as part of init().

Performant Go
=============

1.  When deciding whether to use an interface or a struct: prefer a struct if there will be more than 100 instances. **Justification**: Interfaces are significantly more expensive in both CPU and memory. Exception: Heterogeneous but identically-used objects (such as AST Nodes) may be best represented with an interface. Single-implementation interfaces are bad for small and many data structures.
2.  Prefer fewer channels. **Justification**: Channels [are slow](http://w-dev-blog.appspot.com/posts/2015/03/05/performant-concurrent-queue/index.html) and it’s nearly impossible to guarantee you won’t leave [zombie goroutines](https://groups.google.com/forum/#!topic/golang-nuts/uiySuH8_3Y4) lying around. Channels are threadsafe queues; use them where you might use a queue. Don’t shoehorn them in just because they exist. If you need a high-performance queue, there are [alternatives](https://github.com/Workiva/go-datastructures/blob/master/queue/queue.go).
3.  Avoid defer in tight loops. Don’t use defer inside of a function that you expect to be called in large numbers and in quick succession. Defer also has a nontrivial memory overhead.
4.  Performance profiling is easier in Linux due to OS X [kernel bug](https://github.com/golang/go/issues/6047).
