# plugins

A central repository for [Spinnaker](https://spinnaker.io) plugins.

# Usage

Add the repository to your Spinnaker service configs:

```yaml
spinnaker:
  extensibility:
    repositories:
      core:       # you can name this repository whatever you want
        enabled: true
        url: https://raw.githubusercontent.com/spinnaker/plugins/master/repository/plugins.json
```

Then add the plugins you want to install to your various services:

```yaml
spinnaker:
  extensibility:
    plugins:
      netflix.example:    # This is the plugin ID
        enabled: true
        # you may need additional configuration for the plugin
```

By default, Spinnaker will install the latest plugin version that is supported by your Spinnaker deployment.
If you need to install a specific version, you can pin the version via an additional `version` property.

# Contributing

This repository does not include source code for any plugins, it is only meant to be a centrally-located repository that points to plugins.
As a plugin developer, you are responsible for the build and artifact storage: Github Releases are a great, free option for serving your plugin artifacts.

## Submitting a new plugin

When you build a new plugin, a `plugin-info.json` file will be created as part of your build artifacts which should be added to the bottom of the `repository/plugins.json` file in this repository.
This file will look something like this:

```json
{
  "id": "netflix.example",
  "description": "An example plugin for the Plugins Guide",
  "provider": "example@example.com",
  "homepage": "https://netflix.com",
  "repository": {
    "type": "git",
    "url": "https://example.com/example.git"
  },
  "releases": [
    {
      "version": "0.0.2",
      "date": "2020-05-07T22:14:42.247Z",
      "requires": "orca>=0.0.0,deck>=0.0.0",
      "url": "https://example.com/binaries/0.0.2.zip",
      "sha512sum": "c6eb095b481e9beb81e5d40e0d5f650bf2f54a810f260f1f8f44ca52ead819febfccdeba60548c44eabd5c650be0f3c47768a932c9f9d44919d3dd776ae8f93e"
    }
  ]
}
```

### Requirements

- All plugins must have an `id` and `description`.
- All plugin releases must use an unauthenticated, secure (HTTPS) artifact URL.
- All plugin releases must includ a valid `sha512sum` value.

One of the following contact methods must be available:

- `provider`: The email address or github profile of the user providing this plugin.
- `homepage`: The plugin homepage (could be a github repository).

## Releasing a new plugin version

Follow the same steps for submitting a new plugin, but only update your plugin's `releases` field, adding the latest release to the bottom of the list.

## Deprecating an old plugin version

We do not currently allow for old plugin versions to be removed from this repository while we evaluate how to ensure existing users are not 

# FAQs

## Can closed-source plugins be submitted?

Yes.

When submitting a closed-source plugin, you will still be expected to submit plugin information for documentation and so-on.

## Are these plugins reviewed or audited by the Spinnaker Foundation?

No. 

This repository is curated by the community and does not go through vulnerability scans, security audits, or code reviews officially by the Spinnaker Foundation.
