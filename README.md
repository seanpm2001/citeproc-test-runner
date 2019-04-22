[Citeproc Test Runner](https://www.npmjs.com/package/citeproc-test-runner) (CTR for short) is a productivity tool for maintaining citation styles in the popular [Citation Style Language](https://citationstyles.org/) (CSL) used by [Zotero](https://www.zotero.org/), [Mendeley](https://www.mendeley.com/), [Jurism](https://juris-m.github.io/) and other projects. The script assumes that [node](https://nodejs.org/), [java](https://www.java.com/en/download/) and [mocha](https://mochajs.org/) are installed (the last globally with `npm i -g mocha`). With those in place, installation is a one-liner:
``` bash
    npm install -g citeproc-test-runner
```

After installation, starting the program with `cslrun` will spit out instructions pointing back to this document. The remaining steps for setup are explained below. The instructions assume that you have a GitHub account (if you don't yet have an account, you should [create a free account now](https://github.com)).

## Setting up

CTR can build tests for individual styles, using items from a shared public library. This allows style maintainers to quickly confirm that changes subsequently made to the style do not have unwanted side effects.

Tests are identified to a style using its *slug name*, which is the last portion of the style's CSL `id` string. For example, the slug name of the style `http://www.zotero.org/styles/cell` is `cell`.

Tests are written into and run from a local *style tests directory*. To facilitate the sharing of tests among maintainers, this directory should be a clone of *your personal fork* of the [jm-style-tests](https://github.com/Juris-M/jm-style-tests) repository on GitHub:
```bash
    <visit https://github.com/Juris-M/jm-style-tests and fork the repository>
    git clone https://github.com/my-github-name/jm-style-tests.git
```

After cloning your fork of the tests repository, set the path to it in the CTR configuration file, located in the user home directory `~/.cslrun.yaml`:
```yaml
groupID: 2319948
path:
  styletests: /path/to/jm-style-tests
  local: false
  std: false
  src: false
  locale: false
  modules: false
  cslschema: false
  cslmschema: false
```

By default, CTR builds tests from [Jurism Style Test Items](https://www.zotero.org/groups/2319948/jurism_style_test_items?), a public group library (to use a different source library, set its `id` as the `groupID` value in `~/.cslrun.yaml`). To make items available for a given style, join the group and create a top-level collection with the slug name of the style, and add some items to the collection. It is helpful to write a short description of the item to be tests (i.e. within 30 characters or so) into the Abstract field.

To build tests, use `cslrun` with the `-w` option to set the style to be tested, and the `-U` option to update test fixtures from the group library:
```bash
    cslrun -w /path/to/cell.csl -U
```

This will write test fixtures to a subdirectory of the style tests directory, named for the slug name of the style (if the subdirectory does not exist, it will be created). Finish the tests by using `cslrun` with the `a` option to run all tests, and the `-k` option to confirm output:
```bash
    cslrun -w /path/to/cell.csl -a -k
```

With the `-k` option, `cslrun` will show the style output as a failed test, prompting for confirmation that the output is correct. If `Y` is selected for each failing fixture, all tests will pass. Use `ctrl-c` to exit `cslrun` and return to the command line.

To share the tests. check them in and push them to your `jm-style-tests` repository on GitHub:
```bash
    cd /path/to/jm-style-tests
    git commit -m "Add tests for cell" -a
    git push
```

Then jump to GitHub and file a pull request to the main `jm-style-tests` project to have your tests added to the suite.

That's pretty much it. You can rerun the tests against the style anytime. If the `-k` option is omitted, `cslrun` will simply watch the CSL file, revalidating it and rerunning all tests whenever it is saved, to convert any plain text editor into a poorman's CSL IDE:
```bash
    cslrun -w /path/to/cell.csl -a
```

A few other options are available on CTR. See `cslrun -h` for further details.

Enjoy!

FB
