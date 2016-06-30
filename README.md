# solano-npm-publish

Easy publishing of NPM packages from Solano CI builds. 

You can either add the `solano-npm-publish` script to your repository, or since
`<repo-root>/node_modules/.bin` is already in the `$PATH` on Solano CI workers,
you can `npm install` it and issue a `solano-npm-publish` command to publish your
NPM package repositories. Typically this is done in `post_build` 
[setup hook](http://docs.solanolabs.com/Setup/setup-hooks/) like the following:

```yaml
hooks:
  post_build: npm install solano-npm-publish && solano-npm-publish 
```

#### Notes:

  * A `$NPM_TOKEN` environment variable is required to be set. This
value is used as the `_authToken` value when createing a `.npmrc` file.
[Sensitive environment variables](http://docs.solanolabs.com/Setup/setting-environment-variables/#via-config-variables)
should be set with the `solano config` command.
  * An optional `$NPM_REGISTRY` value can be provided. The default 
is `//registry.npmjs.org/`.
  * As is, it will only `npm publish` when **all** Solano CI tests
have passed, it is a build on the "master" branch, the build was
initiated by a webhook (including Github "push" event webhooks), and
if the commit has a suitable semantic version tag.
