# acme

A simple command line tool to manage TLS certificates with ACME-compliant CAs,
which has no third party depedencies.

If you're looking for a package to import in your program, `golang.org/x/crypto/acme`
or `golang.org/x/crypto/acme/autocert` is what you'll want instead.

*This package is a work in progress and makes no API stability promises.*

## Usage

Quick install with `go get -u github.com/google/acme`
or download a pre-built binary from the
[releases page](https://github.com/google/acme/releases).

The release binaries have an additional command, `acme version`,
which reports the release version.

1. You need to have a user account, registered with the CA. This is represented
  by an RSA private key.

  The easiest is to let the `acme` tool generate it for you:

        acme reg -gen mailto:email@example.com

  If you want to generate a key manually:

        mkdir -p ~/.config/acme
        openssl genrsa -out ~/.config/acme/account.key 4096
        acme reg mailto:email@example.com

  The latter version assumes that default `acme` config dir is `~/.config/acme`.
  Yours may vary. Check with `acme help reg`.

  The "mailto:email@example.com" in the example above is a contact argument.
  While some ACME CA may let you register without providing any contact info,
  it is recommended to use one. For instance a CA might need to notify
  cert owners with an update.

2. Agree with the ACME CA Terms of Service.

  Before requesting your first certificate, you may need to agree with
  the terms of the CA. You can check the status of our account with:

        acme whoami

  and look for "Accepted: ..." line. If it says "no", check CA's terms document
  provided as a link in "Terms: ..." field and agree by executing:

        acme update -accept

3. Request a new certificate for your domain.

  The easiest way to do this is:

        acme cert example.com

  The above command will generate a new certificate key (unless one already exists),
  and send a certifcate request. The location of the output files is `~/.config.acme`,
  but depends on your environment. Check with `acme help cert`.

  If you don't want auto-generated cert key, one can always be generated upfront:

        openssl genrsa -out cert.key 2048

  in which case the cert command will look something like this:

        acme cert -k cert.key example.com

  Note that for certificate request command to succeed, it needs to be executed in a way
  allowing for resolving authorization challenges (domain ownership proof). This typically
  means the command should be executed on the same host the domain is served from.

  If the latter is not possible, use `-manual` flag and follow the instructions:

        acme cert -manual example.com


## License

(c) Google, 2015. Licensed under [Apache-2](LICENSE) license.

This is not an official Google product.
