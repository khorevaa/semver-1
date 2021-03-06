Semantic Versioning for Golang
==============================

[![build status](https://hub.blitznote.com/mark/semver/badges/master/build.svg)](https://hub.blitznote.com/mark/semver/commits/master)
[![Codebeat Badge](https://codebeat.co/badges/f4952f49-2c84-423e-af7b-817616e5f48e)](https://codebeat.co/projects/github-com-wmark-semver)
[![Coverage Status](https://coveralls.io/repos/wmark/semver/badge.png?branch=master)](https://coveralls.io/r/wmark/semver?branch=master)
[![GoDoc](https://godoc.org/github.com/wmark/semver?status.png)](https://godoc.org/github.com/wmark/semver)

A library for parsing and processing of *Versions* and *Ranges* in:

* [Semantic Versioning](http://semver.org/) (semver) v2.0.0 notation
  * used by npmjs.org, pypi.org…
* Gentoo's ebuild format
* NPM

Does not use regular expressions.

```bash
$ go test -run=XXX -benchmem -bench=.

BenchmarkHashicorpNewVersion-24          1000000  1504 ns/op   432 B/op   5 allocs/op
BenchmarkBlangMake-24                    2000000   753 ns/op    96 B/op   3 allocs/op
BenchmarkSemverNewVersion-24            20000000   108 ns/op     0 B/op   0 allocs/op ←

BenchmarkHashicorpNewConstraint-24        200000  6450 ns/op  1680 B/op  18 allocs/op
BenchmarkBlangParseRange-24              1000000  1715 ns/op   400 B/op  10 allocs/op
BenchmarkSemverNewRange-24               5000000   310 ns/op     0 B/op   0 allocs/op ←
```

Licensed under a [BSD-style license](LICENSE).

Usage
-----
```bash
$ go get -v -d github.com/wmark/semver
```

```go
import "github.com/wmark/semver"

v1, err := semver.NewVersion("1.2.3-beta")
v2, err := semver.NewVersion("2.0.0-alpha20140805.456-rc3+build1800")
v1.Less(v2) // true

r1, err := NewRange("~1.2")
r1.Contains(v1)      // true
r1.IsSatisfiedBy(v1) // false (rejects pre-releases: alphas, betas…)
```

Also check the [GoDocs](http://godoc.org/github.com/wmark/semver)
and [Gentoo Linux Ebuild File Format](http://devmanual.gentoo.org/ebuild-writing/file-format/),
[Gentoo's notation of dependencies](http://devmanual.gentoo.org/general-concepts/dependencies/).

Please Note
-----------

It is, ordered from lowest to highest:

    alpha < beta < pre < rc < (no release type/»common«) < r (revision) < p

Therefore it is:

    Version("1.0.0-pre1") < Version("1.0.0") < Version("1.0.0-p1")

Usage Note
----------

Most *NodeJS* authors write **~1.2.3** where **>=1.2.3** would fit better.
*~1.2.3* is ```Range(">=1.2.3 <1.3.0")``` and excludes versions such as *1.4.0*,
which almost always work.

Contribute
----------

Please open issues with minimal examples of what is going wrong. For example:

    Mark, this does not work but I feel that it should:

    ```go
    v, err := semver.Version("17.1o0")
    # yields an error
    # I expected 17.100
    ```

Please write a test case with your expectations, if you ask for a new feature.

    I'd like **semver.Range** to support interface **expvar.Var**, like this:

    ```go
    Convey("Range implements interface's expvar.Var…", t, func() {
      r, _ := NewRange("1.2.3")
      
      Convey("String()", func() {
        So(r.String(), ShouldEqual, "1.2.3")
      })
    })
    ```

Pull requests are welcome.
Please add your name and email address to a file *AUTHORS* and/or *CONTRIBUTORS*.
Thanks!
