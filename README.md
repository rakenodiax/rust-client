# rust-client

A fast, opinionated command line HTTP client.

Fundamentally, `rust-client` is a thin wrapper around Rust's fantastic `reqwest` library. Unlike `curl`, however, it is designed more as a debugging tool. Headers are displayed above the response body, the command line interface is more intuitive than remembering flags, and default en-/decoding behavior.

## Quickstart

`rust-client` can be installed via `cargo`:

```sh
cargo install rust_client
```

Or you can clone the repository:

```sh
git clone https://gitlab.com/rakenodiax/rust-client.git && cd rust-client
cargo install
```

## Performance

The following is a totally unscientific benchmark using `/usr/bin/time` to finely measure memory usage and timing for `rust-client`, `curl`, and Python `http`; on my very old development box. Each app was run 5 times on a warm cache, with the following being the averages:

```sh
> /usr/bin/time -l rc get localhost:8000
GET http://localhost:8000/
HTTP/1.1 200 OK
content-length: 9
date: Tue, 10 Jul 2018 14:21:55 GMT
---
It works!
        0.03 real         0.01 user         0.01 sys
   9494528  maximum resident set size
         0  average shared memory size
         0  average unshared data size
         0  average unshared stack size
      2376  page reclaims
         0  page faults
         0  swaps
         0  block input operations
         0  block output operations
        10  messages sent
        10  messages received
         0  signals received
         4  voluntary context switches
        95  involuntary context switches

> /usr/bin/time -l curl localhost:8000
It works!        0.03 real         0.01 user         0.00 sys
   5005312  maximum resident set size
         0  average shared memory size
         0  average unshared data size
         0  average unshared stack size
      1272  page reclaims
         0  page faults
         0  swaps
         0  block input operations
         0  block output operations
        10  messages sent
        10  messages received
         0  signals received
         7  voluntary context switches
        47  involuntary context switches

> /usr/bin/time -l http localhost:8000
HTTP/1.1 200 OK
content-length: 9
date: Tue, 10 Jul 2018 14:24:20 GMT

It works!

        0.80 real         0.45 user         0.26 sys
  26623181  maximum resident set size
         0  average shared memory size
         0  average unshared data size
         0  average unshared stack size
     33318  page reclaims
         0  page faults
         0  swaps
         0  block input operations
         0  block output operations
        10  messages sent
        10  messages received
        30  signals received
       100  voluntary context switches
      1012  involuntary context switches
```

Note, the measurement of `http` is after warming up the Python runtime using multiple runs of `http`. The following is the initial result:

```sh
HTTP/1.1 200 OK
content-length: 9
date: Tue, 10 Jul 2018 14:23:53 GMT

It works!

        3.75 real         0.46 user         0.36 sys
  26664960  maximum resident set size
         0  average shared memory size
         0  average unshared data size
         0  average unshared stack size
     32776  page reclaims
       535  page faults
         0  swaps
       148  block input operations
         0  block output operations
        14  messages sent
        14  messages received
        29  signals received
       761  voluntary context switches
      1263  involuntary context switches
```

While `rust-client` and `curl` perform similarly, `curl` does not print the same level of information that `rust-client` does.

The test server is the example server from `hyper`'s documentation.

## Contributing

For new features and bug reports, please open an issue on [GitLab](https://gitlab.com/rakenodiax/rust-client/issues/new?issue%5Bassignee_id%5D=&issue%5Bmilestone_id%5D=) or [Github](https://github.com/rakenodiax/rust-client/issues/new).

## Code of Conduct

This project is governed by its [code of conduct](code_of_conduct.md), which all participants (maintainers, contributors, and commenters) must follow.

## TODO
- [x] Have `Command` include request body from `RunConfig`
- [ ] Add `JSON` and `Form` arguments which automatically set headers and serialize appropriately
- [ ] Allow adding arbitrary headers to request
- [ ] Add documentation to major types and functions
- [ ] Flag to disable ansi escaping for output
- [ ] HTML pretty printing
- [ ] encode body content in a specific format (JSON, YAML, etc)
- [ ] decode response based on Content-Type
