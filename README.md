[![Test & Deploy fake-useragent](https://github.com/fake-useragent/fake-useragent/actions/workflows/action.yml/badge.svg?branch=master)](https://github.com/fake-useragent/fake-useragent/actions/workflows/action.yml?query=branch%3Amaster)

# fake-useragent

Up-to-date simple useragent faker with real world database.

## Features

- Data is pre-downloaded from [techblog.willshouse.com](https://techblog.willshouse.com/2012/01/03/most-common-user-agents/) and the data is part of the package
- Retrieves user-agent strings locally
- Supports Python 3.x

### Installation

```sh
pip install fake-useragent
```

Or if you have multiple Python / pip versions installed, use `pip3`:

```sh
pip3 install fake-useragent
```

### Usage

Simple usage example, see below for more examples:

```py
from fake_useragent import UserAgent
ua = UserAgent()

# Get a random browser user-agent string
print(ua.random)

# Or get user-agent string from a specific browser
print(ua.chrome)
# Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36
print(ua.google)
# Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_4) AppleWebKit/537.13 (KHTML, like Gecko) Chrome/24.0.1290.1 Safari/537.13
print(ua['google chrome'])
# Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36
print(ua.firefox)
# Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
print(ua.ff)
# Mozilla/5.0 (X11; Linux x86_64; rv:102.0) Gecko/20100101 Firefox/102.0
print(ua.safari)
# Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.2 Safari/605.1.15
```

#### Additional usage

Additional features that fake-useragent now offers since v1.2.0.

If you want to specify your own browser list, you can do that via the `browsers` argument (default is: `["chrome", "edge", "firefox", "safari"]`).  
This example will only return random useragents from Edge and Chrome:

```py
from fake_useragent import UserAgent
ua = UserAgent(browsers=['edge', 'chrome'])
ua.random
```

_Note:_ Fakeuser-agent knowns about: Chrome, Edge, Firefox and Safari. Other browsers are not popular enough and aren't part of our dataset we use.

---

If you want to specify your own operating systems, you can do that via the `os` argument (default is: `["windows", "macos", "linux"]`).  
In this example you will only get Linux useragents back:

```py
from fake_useragent import UserAgent
ua = UserAgent(os='linux')
ua.random
```

---

If you want to return more popular useragent strings, you can play with the `min_percentage` argument (default is: `0.0`, meaning all useragents will match).  
In this example you get only useragents that have a minimum usage percentage of 1.3% (or higher):

```py
from fake_useragent import UserAgent
ua = UserAgent(min_percentage=1.3)
ua.random
```

_Hint:_ Of-course you can **combine all those arguments** to you liking!

### Notes

You can override the fallback string using the `fallback` parameter, in very rare cases something failed:

```py
import fake_useragent

ua = fake_useragent.UserAgent(fallback='your favorite Browser')
# in case if something went wrong, one more time it is REALLY!!! rare case
ua.random == 'your favorite Browser'
```

If you will try to get unknown browser:

```py
from fake_useragent import UserAgent
ua = UserAgent()
print(ua.unknown)
#Error occurred during getting browser: randm, but was suppressed with fallback.
#Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36
```

If you need to safe some attributes from overriding them in UserAgent by `__getattr__` method
use `safe_attrs` you can pass there attributes names.
At least this will prevent you from raising FakeUserAgentError when attribute not found.

For example, when using fake*useragent with `injections <https://github.com/tailhook/injections>`* you need to:

```py
import fake_useragent

ua = fake_useragent.UserAgent(safe_attrs=('__injections__',))
```

Please, do not use if you don't understand why you need this.
This is magic for rarely extreme case.

### Experiencing issues?

Make sure that you using latest version!

```sh
pip install --upgrade fake-useragent
```

Or if that isn't working, try to install the latest package version like this (`1.2.0` is an example, check what the [latest version is on PyPi](https://pypi.org/project/fake-useragent/#history)):

```sh
pip install fake-useragent==1.2.0
```

Check version via the Python console:

```py
import fake_useragent

print(fake_useragent.VERSION)
```

And you are always welcome to post [issues](https://github.com/fake-useragent/fake-useragent/issues).

Please do not forget to mention the version that you are using.

### For Developers

Since GitHub Actions is unable to reach willshouse.com and has Cloudflare protection. We can run the script below to automatically scrape the user-agent strings from the external data source. The script will copy the [JSONlines](https://jsonlines.org/) file to the `src/fake_useragent/data` directory. Execute:

```sh
./update_data_file.sh
```

The data JSON file is part of the Python package, see [pyproject.toml](pyproject.toml). Read more about [Data files support](https://setuptools.pypa.io/en/latest/userguide/datafiles.html).

### Python Virtual Environment

We encourage to use Python virtual environment before installing Pip packages, like so:

```sh
python -m virtualenv env
source env/bin/activate
```

#### Tests

```sh
pip install -r requirements.txt
tox
```

#### Linting

To fix imports:

```sh
pip install -r requirements.txt
ruff --select="I" --fix .
```

### Changelog

- 1.2.1 August 2, 2023

  - Small improvements in the `min_percentage` check
  - Update all Pip package dependencies

- 1.2.0 August 2, 2023

  - Updated browser useragent data
  - Allow filters on browser, OS and usage percentage
  - Update the cache scraper to scape the new data source for user-agent strings
  - Adapted the code to work with the new JSON data format
  - Parameter `use_external_data=True` is removed. If you use this parameter, just remove it!

- 1.1.3 March 20, 2023

  - Update dependencies

- 1.1.2 February 8, 2023

  - Security fixes

- 1.1.1 December 4, 2022

  - Remove whitespaces from user agent strings, this is a patch release

- 1.1.0 November 26, 2022

  - Add `pkg_resource` as fallback mechanism in trying to retrieve the local JSON data file

- 1.0.1 November 10, 2022

  - Add `importlib-metadata` & `importlib-resources` as dependencies
  - Check on specific Python version regarding the importlib resources (python v3.10 or higher) in order to have `files()` working
  - `importlib_metadata` should now also work on Python version before 3.8
  - Remove obsolete `MANIFEST.in` file

- 1.0.0 November 17, 2022

  - Make the JSON Lines data file part of the Python package, data is retrieved locally
    - Extend the `myproject.toml` file with `package-data` support
  - Remove centralized caching server implementation
  - Make real unit-tests which should run reliable, fast, independent and without Internet connection

- 0.1.14 November 5, 2022

  - Improve code quality standards using modern Python >=3.7 syntax
  - Migrated to `pyproject.toml` build system format + syntax check
  - Add additional classifiers to the toml file
  - Improved `tox.ini` file
  - Improved GitHub Actions job using pip cache
  - And various small fixes

- 0.1.13 October 21, 2022

  - Implement `browsers` argument, allowing you to override the browser names you want to use
  - Fix browser listing of Internet Explorer and Edge
  - Don't depend on w3schools.com anymore
  - Clean-up data (temp) file format
  - Update fallback cache server URL / use JSON Lines as file format
  - Move to GitHub Actions instead of Travis
  - Using [`black`](https://pypi.org/project/black/) Python formatter in favour of Flake

- 0.1.12 March 31, 2022

  - forked

- 0.1.11 October 4, 2018

  - moved `s3 + cloudfront` fallback to `heroku.com`, cuz someone from Florida did ~25M requests last month

- 0.1.10 February 11, 2018

  - Minor fix docs `cloudfront` url

- 0.1.9 February 11, 2018

  - fix `w3schools.com` renamed `IE/Edge` to `Edge/IE`
  - moved `heroku.com` fallback to `s3 + cloudfront`
  - stop testing Python3.3 and pypy

- 0.1.8 November 2, 2017

  - fix `useragentstring.com` `Can't connect to local MySQL server through socket`

- 0.1.7 April 2, 2017

  - fix broken README.rst

- 0.1.6 April 2, 2017

  - fixes bug `use_cache_server` do not affected anything
  - `w3schools.com <https://www.w3schools.com/browsers/browsers_stats.asp>`\_ moved to `https`
  - `verify_ssl` options added, by default it is `True` (`urllib.urlopen` ssl context for Python 2.7.9- and 3.4.3- is not supported)

- 0.1.5 February 28, 2017

  - added `ua.edge` alias to Internet Explorer
  - w3schools.com starts displaying `Edge` statistic
  - Python 2.6 is not tested anymore
  - `use_cache_server` option added
  - Increased `fake_useragent.settings.HTTP_TIMEOUT` to 5 seconds

- 0.1.4 December 14, 2016

  - Added custom data file location support
  - Added `fallback` browser support, in case of unavailable data sources
  - Added alias `fake_useragent.FakeUserAgent` for `fake_useragent.UserAgent`
  - Added alias `fake_useragent.UserAgentError` for `fake_useragent.FakeUserAgentError`
  - Reduced `fake_useragent.settings.HTTP_TIMEOUT` to 3 seconds
  - Started migration to new data file format
  - Simplified a lot 4+ years out of date code
  - Better thread/greenlet safety
  - Added verbose logging
  - Added `safe_attrs` for prevent overriding by `__getattr__`

- 0.1.3 November 24, 2016

  - Added hosted data file, when remote services is unavailable
  - Raises `fake_useragent.errors.FakeUserAgentError` in case when there is not way to download data
  - Raises `fake_useragent.errors.FakeUserAgentError` instead of `None` in case of unknown browser
  - Added `gevent.sleep` support in `gevent` patched environment when trying to download data

- X.X.X xxxxxxx xx, xxxx
  - xxxxx ?????

### Authors

You can visit [authors page](https://github.com/fake-useragent/fake-useragent/blob/master/AUTHORS).
