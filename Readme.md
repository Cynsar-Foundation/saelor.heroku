## Custom Heroku Buildpack for Saleor

This buildpack is designed to handle a couple of specific tasks that are necessary for deploying a Saleor project on Heroku:

1. It writes the contents of the `GOOGLE_CREDENTIALS` Heroku config var to a file named `google-credentials.json`.
2. While the `release`, `web`, and `celeryworker` commands should typically be in a `Procfile`, this buildpack also demonstrates how to define them in a buildpack's `release` script. This can be useful in scenarios where you do not want to include a `Procfile` in the main codebase.

### How to Use

#### 1. Prepare Your Heroku App

Ensure you have Heroku CLI installed and you're logged in. If not, follow the [Heroku CLI documentation](https://devcenter.heroku.com/articles/heroku-cli) to get set up.

#### 2. Set Up Buildpacks

For this example, let's assume you're starting fresh. You need to add both the standard Python buildpack and your custom one:

```bash
heroku buildpacks:add heroku/python
heroku buildpacks:add https://github.com/Cynsar-Foundation/saelor.heroku
```

The order matters! You want the Python buildpack to execute first. You can verify the order with:

```bash
heroku buildpacks
```

If needed, adjust the order using the `--index` option.

#### 3. Set GOOGLE_CREDENTIALS

The buildpack expects the Google credentials to be present in an environment variable. You can set this up with:

```bash
heroku config:set GOOGLE_CREDENTIALS='your_credentials_here'
```

#### 4. Deploy to Heroku

Now, when you deploy to Heroku, it will first use the Python buildpack to set up the environment and then the custom buildpack to handle the Google credentials and potentially the release process.

```bash
git push heroku master
```

### Behind the Scenes

The main logic of this buildpack is encapsulated in the `bin/compile` script. It checks for the presence of `GOOGLE_CREDENTIALS` and then writes the credentials to the `google-credentials.json` file.

### Notes

- It's generally recommended to use a `Procfile` for defining processes like web and worker commands. However, this buildpack provides an example of how it can be done directly within the buildpack for demonstration purposes.
- Always ensure that sensitive information like Google credentials is managed securely.

---

That's a simple README covering the process. Adjust as needed for clarity or additional details relevant to your audience. If you're hosting this on a platform like GitHub, consider using markdown styling for better readability.