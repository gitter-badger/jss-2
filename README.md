[![Build Status](https://travis-ci.org/danvk/jss.svg?branch=master)](https://travis-ci.org/danvk/jss)

jss
===

[![Join the chat at https://gitter.im/danvk/jss](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/danvk/jss?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

jss is a JSON processing command line tool (like jq).

Unlike [jq](http://stedolan.github.io/jq/), its selection language is
[JSONSelect](http://jsonselect.org/#overview), which is based on CSS
selectors. No need to learn an ad-hoc language for processing your JSON files.
Just use one you already know! Your time with jss won't be wasted—it will make
you better at writing CSS selectors.


Usage
-------

Install:

    $ pip install jss

Sample JSON file for demos:

    $ cat file.json

```json
{
  "foo": [
    "bar",
    {
      "baz": "quux"
    }
  ],
  "wut": {
    "name": "foo",
    "metadata": {
      "owner": "danvk",
      "blah": "whatever"
    }
  },
  "name": "dan"
}
```


Pull out all values with key "name", from anywhere in the JSON.

    $ jss .name file.json

```json
"foo"
"dan"
```

Remove fields named "metadata", wherever they occur (JSON→JSON transform):

    $ jss -v .metadata file.json

```json
{
  "foo": [
    "bar",
    {
      "baz": "quux"
    }
  ],
  "wut": {
    "name": "foo"
  },
  "name": "dan"
}
```

Keep only fields named "name", plus their ancestors (JSON→JSON transform):

    $ jss -k .name file.json

```json
{
  "wut": {
    "name": "foo"
  },
  "name": "dan"
}
```

Keep only top-level entries with "whatever" in some value underneath them (JSON→JSON transform using jQuery-style selectors):

    $ jss -k ':root>*:has(:contains("whatever"))' file.json

```json
{
  "wut": {
    "name": "foo",
    "metadata": {
      "owner": "danvk",
      "blah": "whatever"
    }
  }
}
```
