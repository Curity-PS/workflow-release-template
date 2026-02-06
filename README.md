# Workflow-release-template

This template contains boilerplate code for the gradle release. The Gradle release workflow under GitHub Actions would 
quickly create a new release.

Anyone with access to the template repository can create a new repository based on the template 
with the same directory structure, branches, and files.

# How to use template repository

1. Create a new repository.
2. Choose the Repository Template from the dropdown.

After you make your repository a template, anyone with access to the repository can generate a new repository with the same directory structure and files as your default branch.

# Plugin development

Before starting with the plugin development, Make sure you have right settings in place.

1. Please check and update _`Dependencies`_, _`Version`_, _`Description`_  iN `build.gradle` file.
2. _`rootProject.name`_ in `settings.gradle` file.

## Environment Configuration

The plugin development workflow supports configuration through a `.env` file for convenience. This allows you to set environment variables without exporting them in your shell.

1. Copy the `.env-example` file to `.env`:
   ```bash
   cp .env-example .env
   ```

2. Update the values in `.env`:
   - `IDSVR_HOME`: Path to your Curity Identity Server installation directory
   - `LICENSE_KEY`: Your Curity Identity Server license JWT

**Note:** The `.env` file is already included in `.gitignore` and will not be committed to version control.

After the plugin development , please follow the below steps to make release.

1. Navigate to the Actions tab. 
2. On the left hand side , choose `_Gradle release workflow_` under _`All workflows`_ . 
3. You would see _`Run workflow`_ button on the right side to run the release.

# Deploy to local server

The Curity Plugin Development Gradle plugin provides a task to deploy your plugin to a local server. It will collect the runtime dependencies into the plugins folder of your `IDSVR_HOME`.

Make sure you have configured the `IDSVR_HOME` environment variable (either in your shell or in a `.env` file):

```bash
./gradlew deployToLocal
```

# Create release folder

To compile the plugin and collect all the dependencies, run this task:

```bash
./gradlew createReleaseDir
```

The result will be in `build/release`
