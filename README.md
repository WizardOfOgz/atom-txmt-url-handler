# Atom Txmt URL Handler

A handler to open txmt:// URLs in Atom editor on OSX.

This project is simply a clone of [Atom Handler](https://github.com/WizardOfOgz/atom-handler) with a change in the protocol which it handles (`txmt://` instead of `atm://`).

Atom Txmt URL Handler registers itself to handler links with the `txmt://` protocol, similar to the protocol TextMate handles (txmt://). Instead of opening URLs which match that protocol in TextMate we open them in Atom.

## Prerequisites

- Install Atom Shell Commands. Within Atom select from the menu: `Atom > Install Shell Commands`

## Installation

- Download the latest release [(atom-txmt-url-handler.app.zip)](https://github.com/WizardOfOgz/atom-txmt-url-handler/releases/download/1.0.0/atom-txmt-url-handler.app.zip) and unzip it.
- Move atom-txmt-url-handler.app into your `/Applications` directory.
- Open the application which will register the handler and exit immediately.

Opening the application in the last step may be prevented by OS security. If that happens go to `System Preferences > Security & Privacy`. You should see a message about "atom-txmt-url-handler" being blocked. Click the button which says "Open Anyway".

## Usage

Atom Txmt URL Handler will handle URLs which match the [TextMate URL scheme](http://blog.macromates.com/2007/the-textmate-url-scheme/) and take the following format:

`txmt://open?url=file://<file_path>[&line=<line>[&column=<column>]]`

Opening a URL with this format will open the given file in the Atom editor and place the cursor at the beginning of the line number (if given). Note that the column option does not seem to be supported by the Atom command line utility. If a column is given then Atom simply ignores it. The option is supported for compatibility with TextMate and Sublime Text URLs.

### Examples:
- `txmt://open?url=file:///path/to/other`
- `txmt://open?url=file:///path/to/other&line=42`
- `txmt://open?url=file:///path/to/file&line=42&column=7`
- `txmt://open?url=file://%2Fpath%2Fto%2Fother  # URL-encoded slashes (/)`

These URLs may be opened from a browser or from the command line using the system `open` command.

## URL Encoding

Note that The file path may contain the following URL-encoded values.

|name|character|URL encoding|
|---|---|---|
|slash|`/`|`%2F`|
|space|` `|`%20` OR `+`|
|plus|`+`|`%2B`|

Plus-signs are legal characters in file names and _must_ be escaped as `%2B` since an unescaped `+` will be transformed into a space character. We highly discourage using plus-signs in file names since support for it varies across systems.

Spaces in file names must likewise be escaped. These are also discouraged since many scripts and applications do not handle them properly.

## Known Limitations

Currently this application relies on the `atom` CLI utility being accessible at `/usr/local/bin/atom` in order to open the files. If there is a better way of finding the CLI utility, please let me know or open a pull request.

## Integrating with BetterErrors

Install [Better Errors](https://github.com/charliesome/better_errors) in your Rails application and add the following code to an initializer (or any other location where it will be executed when your application is loaded.)

```ruby
if defined? BetterErrors
  BetterErrors.editor = "txmt://open?url=file://%{file}&line=%{line}"
end
```

## Integrating with PHP Xdebug

Install [Xdebug](http://xdebug.org) for PHP, add the following to your php.ini, and restart your webserver.

```
xdebug.file_link_format="txmt://open?url=file://%f&line=%l"
```

## License

Atom Txmt URL Handler is released under the [BSD 3-Clause license](https://github.com/WizardOfOgz/atom-txmt-url-handler/blob/master/LICENSE).
