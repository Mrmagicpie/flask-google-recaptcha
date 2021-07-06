# Flask Google ReCaptcha

[![PyPI pyversions](https://img.shields.io/pypi/pyversions/ansicolortags.svg)](https://pypi.python.org/pypi/ansicolortags/)

The Working Google ReCaptcha implementation for Flask without Flask-WTF.

Can also be used as standalone

---

## Install

```shell
pip install Flask-GoogleReCaptcha

# or

pip install git+https://github.com/AndersonFirmino/flask-google-recaptcha.git

# If you are using pipenv
pipenv install Flask-GoogleReCaptcha
```

This implementation is pure python and has no dependencies from third parties. Works in both Python2 and Python3. It also has Google App Engine (GAE) support!


# Usage

### Implementation

```python
from flask import Flask
from flask_google_recaptcha import GoogleReCaptcha

app = Flask(__name__)
recaptcha = GoogleReCaptcha(app=app)

# or

recaptcha = GoogleReCaptcha()
recaptcha.init_app(app)
```

## Templating

Inside of the form you want to protect, include the tag: **{{ recaptcha }}**. This will insert the code automatically. 

```html
<form method="post" action="/submit">
    ... your field
    ... your field

    {{ recaptcha }}

    [submit button]
</form>
```

**Known issue:** If you have defined your own app context_processor this may not work. In order to fix, combine the code below with your context_processor.

```python
# This is used so you do not have to use "{{ recaptcha|safe }}"
from jinja2 import Markup

@app.context_processor
def mycontext():
    return {"recaptcha": Markup(self.get_code()))
```

## Verifying a captcha

In the view that will validate the captcha:

```python
from flask import Flask
from flask_google_recaptcha import GoogleReCaptcha

app = Flask(__name__)
recaptcha = GoogleReCaptcha(app=app)

@app.route("/submit", methods=["POST"])
def submit():

    if recaptcha.verify():
        # SUCCESS
        pass
    else:
        # FAILED
        pass
```

Remember to set SITE_KEY and SECRET_KEY. If you don't, the captcha will not render.

## API

| Function | Return |
| ------ | ------ |
| GoogleReCaptcha | Base class, initialize with your app and Site/Secret keys. |
| GoogleReCaptcha.get_code() | Returns the HTML string for the captcha. |
| {{ recaptcha }} | Returns a template-safe string for use in Jinja templating |
| GoogleReCaptcha.verify() | Verify a captcha response. Returns bool |


<!--
**GoogleReCaptcha(app, site_key, secret_key, is_enabled=True)**

**recaptcha.get_code()**

Returns the HTML code to implement. But you can use
**{{ recaptcha }}** directly in your template

**recaptcha.verfiy()**

Returns bool

-->

# Config

Flask-ReCaptcha is configured through the standard Flask config.
These are the available options:

## Required

| Option | Type/Definition |
| ------ | ------ |
| RECAPTCHA_ENABLED | Bool - Defaults to True, False will bypass validation. |
| RECAPTCHA_SITE_KEY | Public Key |
| RECAPTCHA_SECRET_KEY | Private Key |

## Optional

| Option | Type/Definition |
| ------ | ------ |
| RECAPTCHA_THEME | String - The theme can be 'light'(default) or 'dark' |
| RECAPTCHA_TYPE | String - Type of recaptcha can be 'image'(default) or 'audio' |
| RECAPTCHA_SIZE | String - Size of the image can be 'normal'(default) or 'compact' | 
| RECAPTCHA_TABINDEX | Int - Tabindex of the widget can be used if the page uses tabidex to make navigation easier. Defaults to 0 |

<!--
**RECAPTCHA_ENABLED**: Bool - True by default, when False it will bypass validation

**RECAPTCHA_SITE_KEY** : Public key

**RECAPTCHA_SECRET_KEY**: Private key

The following are **Optional** arguments.

**RECAPTCHA_THEME**: String - Theme can be 'light'(default) or 'dark'

**RECAPTCHA_TYPE**: String - Type of recaptcha can be 'image'(default) or 'audio'

**RECAPTCHA_SIZE**: String - Size of the image can be 'normal'(default) or 'compact'

**RECAPTCHA_TABINDEX**: Int - Tabindex of the widget can be used, if the page uses tabidex, to make navigation easier. Defaults to 0
-->

Small code example for these options :D

```python
app.config['RECAPTCHA_ENABLED'] = True
app.config['RECAPTCHA_SITE_KEY'] = ""
app.config['RECAPTCHA_SECRET_KEY'] = ""
app.config['RECAPTCHA_THEME'] = "dark"
app.config['RECAPTCHA_TYPE'] = "image"
app.config['RECAPTCHA_SIZE'] = "compact"
app.config['RECAPTCHA_TABINDEX'] = 10
```

---

Anderson Araujo (coderpy) :snake:
