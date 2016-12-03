# Installation

Config the **bin-dir** path inside your ```composer.json``` file:

```json
"config": {
    "bin-dir": "bin"
}
```

Next, install the package with composer:

	composer require craftsman/cli

Install the environment variables:

    cp path/to/vendor/craftsman/cli/.craftsman.example .craftsman

If you configure the bin directory you can run the command like this:

    php bin/craftsman

If you don't, Craftsman should be listed as a vendor binary:

    php path/to/vendor/bin/craftsman

---
