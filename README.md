# jsoo-react

[![Actions Status](https://github.com/jchavarri/jsoo-react/workflows/CI/badge.svg)](https://github.com/jchavarri/jsoo-react/actions)

Bindings to [React](https://reactjs.org/) for [js_of_ocaml](ocsigen.org/js_of_ocaml/), including JSX ppx.

Adapted from [ReasonReact](https://github.com/reasonml/reason-react/).

`jsoo-react` allows to use React from OCaml, but it is still at the **experimental** phase: there is no published version in [opam](opam.ocaml.org/) yet, and the library is expected to break backwards compatibility often.

Bug reports and contributions are welcome!

## Getting started

### New project

For new projects, the best way to start is by using [Spin](https://github.com/tmattio/spin) with the [`spin-jsoo-react` template](https://github.com/tmattio/spin-jsoo-react/).

1. First, install Spin [following the instructions](https://github.com/tmattio/spin#installation).

2. Then run:

    ```bash
    spin new https://github.com/tmattio/spin-jsoo-react.git
    ```

    After that, check the newly created project readme to get started.

### Existing project

1. Install the `jsoo-react` package and [gen_js_api](https://github.com/LexiFi/gen_js_api) dependency:

    ```bash
    opam pin add -y gen_js_api https://github.com/jchavarri/gen_js_api.git#typ_var
    opam pin add -y jsoo-react https://github.com/jchavarri/jsoo-react.git
    ```

2. Add `jsoo-react` library and ppx to [dune](https://dune.readthedocs.io/en/stable/) file of your executable JavaScript app:

    ```
    (executables
    (names index)
    (modes js)
    (libraries jsoo-react.lib)
    (preprocess
      (pps jsoo-react.ppx)))
    ```

3. Provision React.js library

    `jsoo-react` does not make any assumptions about how you will load React.js in your application. There are 2 ways of doing so:

    #### With Webpack (or any JavaScript bundler)

    If you want to use Webpack, Rollup, Parcel or any other JavaScript bundler, include a file `react-requires.js` in your application source folder with the following content:

    ```js
    joo_global_object.React = require('react');
    joo_global_object.ReactDOM = require('react-dom');
    ```

    Then add it to your application `dune` file so it can be linked:

    ```
    (executables
    ...
    (js_of_ocaml
      (javascript_files react-requires.js)))
    ```

    To see an example of this approach, check the [example](example) folder.

    Note that at this moment, `jsoo-react` is compatible with **React 16**, so be sure to have the appropriate constraints in your `package.json`.

    #### With `<script>` tags

    If you are not using any JavaScript bundlers, and just loading the `bc.js` artifact generated by Dune, you can just add the following HTML tags to your `index.html` page:

    ```html
    <script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
    <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>
    ```

    Make sure they are loaded before your application `bc.js` artifact.

## Contributing

Take a look at our [Contributing Guide](CONTRIBUTING.md).

## Acknowledgements

Thanks to the authors and maintainers of ReasonReact, in particular [@rickyvetter](https://github.com/rickyvetter) for his work on the v3 of the JSX ppx.

Thanks to the authors and maintainers of Js_of_ocaml, in particular [@hhugo](https://github.com/hhugo) who has been answering many many questions in GitHub threads.

Thanks to the Lexifi team for creating and maintaining [gen_js_api](https://github.com/LexiFi/gen_js_api).

Thanks to [@tmattio](https://github.com/tmattio/) for creating Spin and the jsoo-react template :raised_hands:

And thanks to the team behind React.js! What an amazing library :)
