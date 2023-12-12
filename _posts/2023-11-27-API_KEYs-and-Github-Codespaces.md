---
author: Raman Adlakha
bibliography: "D:.bib"
categories: note
image: Teacher.jpg
layout: post
tags: codespaces github
title: API_KEYs and GitHub Codespaces
---

Here are step-by-step instructions for storing an API key securely in a
GitHub Codespaces environment using environment variables:

1.  **Encrypt the API key using GitHub Secrets.** On GitHub, go to your
    repository settings and select “Secrets and variables” in the
    sidebar. Click Codespaces. Click “New repository secret”. Give it a
    name like “API_KEY”. In the value, paste your raw API key. This
    encrypts the key. Be rest assured as the screen guides you with the
    following statement. Development environment secrets are environment
    variables that are **encrypted**. Secrets are not passed to forks.
2.  **Grant Codespaces access to the secret.** Still in the Secrets
    settings, scroll down to the “Codespaces secrets” section. Click
    “Add secret” and choose the secret you just created. This allows
    Codespaces to access it.
3. **Access the secret in your codespace.** In GitHub Codespaces, 
    secrets are automatically made available as environment variables,
    and there's no need to manually export them in the `.bashrc` or 
    `.env` file. You can directly use them in your code like this:
   ```python
import os

api_key = os.getenv('API_KEY')   
```
	In this case, `API_KEY` is the name of the secret you've stored
    in GitHub Secrets.
4. **Write code to access the secret.** You can now write code to 
    access the API_KEY environment variable. For example, if you are 
    using Python, you can access the environment variable using the 
    following code: 
```python
import os; 
api_key = os.environ['API_KEY1']
```

I wrote this as a quick reminder to myself. Hope it helps you too. There
will be more small how-tos added here as I write them.
