# Installation

Add the **bin-dir** path inside your ```composer.json``` file:

```json
"config": {
    "bin-dir": "bin"
}
```

Next, install the package with composer:

	composer require dsv/craftsman

Install the environment variables:

    cp path/to/vendor/craftsman/utilis/.env.example .env

If you configure the bin directory you can run the command like this:

    php bin/craftsman

If you don't, this package should be listed as a vendor binary:

    php path/to/vendor/bin/craftsman

---
