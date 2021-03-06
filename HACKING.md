Developing for this project
====

Testing
---

Tests are written using Go's testing framework and can be run with the standard
`go test` tool.  If you prefer to use the Makefile, `make test EXTRAGOARGS=-v`
will run tests in verbose mode.

You need some external resources in order to run the tests, as described below:

1. A firecracker binary (tested with 0.10.1), but any recent version should
   work.  Must either be installed as `./firecracker` or the path must be
   specified through the `FC_TEST_BIN` environment variable.
2. Access to hardware virtualization via `/dev/kvm` (ensure you have mode
   `+rw`!)
3. An uncompressed Linux kernel binary that can boot in Firecracker VM (Must be
   installed as `./vmlinux`)
4. A tap device owned by your userid (Must be either named `fc-test-tap0` or
   have the name specified with the `FC_TEST_TAP` environment variable; try
   `sudo ip tuntap add fc-test-tap0 mode tap user $UID` to create `fc-test-tap0`
   if you need to create one)
5. A root filesystem image installed (Must be named `root-drive.img`; can be
   empty, create it something like
   `dd if=/dev/zero of=drive-2.img bs=1k count=102400`)

With all of those set up, `make test EXTRAGOARGS=-v` should create a Firecracker
process and run the Linux kernel in a MicroVM.

Regenerating the API client
---

The API client can be generated using the
[Go swagger implementation](https://goswagger.io/). To do so, perform the
following:

1. Update `client/swagger.yaml`
3. Run `go generate`
4. Figure out what broke and fix it.
