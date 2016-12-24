# portatheme

> Small, opinionated theme bundler

[![Travis](https://img.shields.io/travis/spacedoc/portatheme.svg?maxAge=2592000)](https://travis-ci.org/spacedoc/portatheme) [![npm](https://img.shields.io/npm/v/portatheme.svg?maxAge=2592000)](https://www.npmjs.com/package/portatheme)

## Installation

```bash
npm install portatheme
```

## Setup

Themes operate with these assumptions:

- Pages are dynamically generated by the consumer of a theme.
- A set of layouts and partials are provided by the theme.
- Static assets (CSS, JavaScript, and images) include a mix of consumer- and theme-provided.
- CSS is compiled from Sass.
- JavaScript is compiled from Webpack with Babel.

A minimal theme is structured like this:

```txt
- theme/
  - assets/
  - js/
    - index.js
  - sass/
    - index.scss
  - templates/
    - default.pug
```

Let's break it down.

- `assets` includes static assets like images and fonts.
- `js` includes JavaScript required by your theme.
- `sass` includes Sass required by your theme.
- `templates` includes Pug templates that will render pages.

Static assets placed at the root of the theme, such as a `robots.txt`, will also be copied.

Only the `templates` folder is required, and the only required file in that folder is `default.pug`.

## Usage

```js
import Theme from 'portatheme';

// Initialize the theme with a file path
// The theme can be in node_modules or a local folder
const theme = new Theme('./kittenTheme');

// Set an output folder for pages and theme assets
theme.outputTo('./dist');

// Create a page by passing a file name and data object
theme.compilePage('index.html', { title: 'Home' }).then(() => console.log('Page built.'));

// Compile theme assets
theme.build().then(() => console.log('Assets compiled.'))
```

## API

### new Theme(location)

Create a new theme.

- **location** (String): path to theme folder. Should be an absolute path, or relative to the current working directory.

#### Theme.outputTo(location)

Define the output folder when building theme assets and pages. Call this before using other theme methods.

- **location** (String): path to output to.

#### Theme.compilePage(dest[, data, layout])

Create a page using one of the theme's layouts, paired with a data object.

- **dest** (String): name of file to output, e.g. `index.html`. The path is relative to the root folder set with `Theme.outputTo()`.
- **data** (Object): object to pass to Pug template.
- **layout** (String): theme template to use. Defaults to `default`.

Returns a Promise which resolves when the page has been written to disk, or rejects if there's an error.

#### Theme.build()

Build the assets of a theme: static files, CSS, and JavaScript.

Returns a Promise which resolves when the build process is done, or rejects if there's an error.

#### Theme.buildAndWatch()

Build the assets of a theme, then watch for changes to theme files and rebuild when necessary.

Returns a Promise which rejects if there's an error. The Promise will never resolve, because file watching goes on indefinitely.

## Local Development

```bash
git clone https://github.com/spacedoc/portatheme
cd portatheme
npm install
npm test
```

## License

MIT &copy; [Geoff Kimball](http://geoffkimball.com)
