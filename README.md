# email-parser

The fastest and lightest email parsing Rust library!\
This library has no dependency by default (and only a small optional one).

## Goal

The goal of this library is to be fully compliant with RFC 5322. However, this library does not intend to support the obsolete syntax because it has been obsolete for 12 years, and it would slow down everything.\
This library supports MIME and will support PGP in the future.

## Example

```rust
let email = Email::parse(
    b"\
    From: Mubelotix <mubelotix@mubelotix.dev>\r\n\
    Subject:Example Email\r\n\
    To: Someone <example@example.com>\r\n\
    Message-id: <6546518945@mubelotix.dev>\r\n\
    Date: 5 May 2003 18:58:34 +0000\r\n\
    \r\n\
    Hey!\r\n",
)
.unwrap();

assert_eq!(email.subject.unwrap(), "Example Email");
assert_eq!(email.sender.name.unwrap(), vec!["Mubelotix"]);
assert_eq!(email.sender.address.local_part, "mubelotix");
assert_eq!(email.sender.address.domain, "mubelotix.dev");
```

## Pay for what you use

Mails can be elaborated. No matter what you are building, you are certainly not using all of its features.\
So why would you pay the parsing cost of header fields you are not using? This library allows you to enable headers you need so that other header values will be parsed as an unstructured header, which is much faster.\
By disabling all header value parsing, this library can parse an entire mail twice faster! But don't worry if you need everything enabled; this library is blazing fast anyway!

## Zero-Copy (almost)

This library tries to avoid usage of owned `String`s as much as possible and is using `Cow<str>` instead.\
Thanks to this method, around 90% of the strings are references.

## Benchmarks

This chart shows the time took to parse a single email.

![Benchmark](https://cdn.discordapp.com/attachments/694923348844609597/789162705494868020/unknown.png)

Run these benchmarks by yourself with `rustup run nightly cargo bench` and `rustup run nightly cargo bench --no-default-features`.\
Tests require a `mail.txt` file containing a raw mail next to the `Cargo.toml`.\
Some libraries suffer from huge performance variations depending on the content of the mail, so this library is not **always** the fastest.

License: MIT
