# Contributing to `messageformat`

We welcome all relevant issues and pull requests, and aim to provide a constructive environment.
If something is not clear and you have looked at our [documentation site](https://messageformat.github.io/), then it probably counts as a bug of some sort.

Messageformat is an [OpenJS Foundation](https://openjsf.org) project, and we follow its [Code of Conduct](https://github.com/openjs-foundation/cross-project-council/blob/master/CODE_OF_CONDUCT.md).

Our current governance is pretty straightforward; our core contributors [Alex Sexton](https://github.com/SlexAxton) and [Eemeli Aro](https://github.com/eemeli) are in charge. If you'd like to have a say too, active participation is a really good way forward.

## Getting Started

The monorepo uses [Lerna](https://lerna.js.org/) for package management. Each package is included in the root package.json as a `file:` dependency, and all the dev dependencies are at the root level. This means that just running `npm install` should get you set up with everything. Note that the dependency binaries are not linked from each package, so you may need to use `lerna run` or `lerna exec` to execute scripts in packages.

## Testing & Linting

We like automated tools.
In addition to Node.js tests, GitHub Actions also lint the code and run the test suite in browser environments provided by [BrowserStack].
To run the browser tests locally, do one of the following:

- Install [ChromeDriver] and run `npm run test:chrome`
- Install [geckodriver] and run `npm run test:firefox`
- Run `npm run test:serve` and then open <http://localhost:5000/test/browser/test.html> in your browser
- Ensure that the `BROWSERSTACK_USER` and `BROWSERSTACK_ACCESS_KEY` env vars are set to a valid account and its access key, and run `npm run test:browsers`

Due to relying on Intl.NumberFormat pretty heavily, not all tests will pass in all environments; some will be skipped.

[browserstack]: https://www.browserstack.com/open-source
[chromedriver]: https://chromedriver.chromium.org
[geckodriver]: https://firefox-source-docs.mozilla.org/testing/geckodriver/

## Contributor License Agreement

This repository is managed by EasyCLA. Project participants must sign the free [OpenJS Foundation CLA](https://github.com/openjs-foundation/easycla) before making a contribution. You only need to do this one time, and it can be signed by [individual contributors](https://github.com/openjs-foundation/easycla#sign-as-an-individual) or their [employers](https://github.com/openjs-foundation/easycla#have-your-company-sign-for-you).

To initiate the signature process please open a PR against this repo. The EasyCLA bot will block the merge if we still need a CLA from you.

You can find [detailed instructions here](https://github.com/openjs-foundation/easycla). If you have issues, please email [operations@openjsf.org](mailto:operations@openjsf.org).

If your company benefits from this project and you would like to provide essential financial support for the systems and people that power our community, please also consider [membership in the OpenJS Foundation](https://openjsf.org/about/join).
