---
title: "TLDR: Generate Django Secret Key"
description: "Generate Django's secret key right from your terminal without needing to risk yourself on any website"
publishDate: 2019-07-12
tags:
  - tldr
  - python
  - django
images:
  - /img/posts/tldr-generate-django-secret-key.png

---

Raise your hand if you never versioned the Django's `SECRET_KEY` at the beginning of a project and needed to generate a new one before going to production.

This **TLDR** is a quick reminder of how to generate a secret key locally, without going to some website on the internet to generate it for you.

Django generates a secret key every time that you create a new project, so this function already exists at its code, and you can access it in this way:

```python
from django.core.management.utils import get_random_secret_key

print(get_random_secret_key())
```

If you don't even want to have the work of stating the Python shell, you can execute this command on the terminal:

```console
python -c 'from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())'
```

{{< tip class="info" >}}
Remember that you need Django installed at the environment to run this command.
{{< /tip >}}

## Update for Python 3.6+

Thanks to [@ChristianHeimes](https://twitter.com/ChristianHeimes) and [@pauloxnet](https://twitter.com/pauloxnet) interaction at [@aclark4life](https://twitter.com/aclark4life) post on twitter. I leaned that starting from Python 3.6 version the lib [secrets](https://docs.python.org/3/library/secrets.html) was added to help with the generation of cryptographically strong random numbers suitable for managing data such as passwords, account authentication, security tokens, and related secrets.

Its a cool **batteries included** solution to generate your `SECRET_KEY` without Django dependency:

```python
import secrets

print(secrets.token_urlsafe())
```

You can also do it from terminal command:

```console
python -c "import secrets; print(secrets.token_urlsafe())"
```

And in case you got curious on how Django does that nowadays guess what? They are using it as well. 🎉

Starting from tag version 3.1.3 this is what `get_random_secret_key` do on background:

```python
import secrets

length = 50
chars = 'abcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*(-_=+)'

secret_key = ''.join(secrets.choice(chars) for i in range(length))

print(secret_key)
```
