# Genesis Sample Task Runner

This is a Gulp workflow for automating the following tasks:

- Auto prefixing
- Compiling Sass partials into style.css
- Minifying unminified .js files and style.css
- Automatic CSS injection and browser reloading for PHP & JS changes using BrowserSync
- CSS optimization
- Packing same CSS media query rules into one
- Generating pixel fallbacks for rem units
- Generating source maps so browser inspector (like Chrome DevTools) shows the partial .scss file(s) from where CSS rules are originating from

for [Genesis Sample](https://github.com/copyblogger/genesis-sample), a child theme of the [Genesis](https://sridharkatakam.com/go/genesis) framework.

## How to use

0. Install WordPress on your localhost if you haven't already. I use [Laravel Valet](https://laravel.com/docs/5.6/valet).

1. Install [Node](https://nodejs.org/download/).

2. Install Gulp CLI globally by running `npm install gulp-cli -g` in the terminal.

3. Download this repo's content and place in your local site's project folder (Ex.: Genesis Sample theme directory).

4. Run `npm install`.

5. Change the values of `siteName` and `userName` in gulpfile.js.

If your local site does not have a SSL, you can comment out the `userName` line and comment out/delete

```
https: {
    key: `/Users/${userName}/.valet/Certificates/${siteName}.key`,
    cert: `/Users/${userName}/.valet/Certificates/${siteName}.crt`
}
```

If it does, adjust the path to your local SSL certificate's key and crt files.

6. Run `gulp`.

7. You might want to load the minified versions of `genesis-sample.js` and `style.css` on the production site before going live.

For this, edit `functions.php` and

a) replace

```
wp_enqueue_script(
    'genesis-sample',
    get_stylesheet_directory_uri() . '/js/genesis-sample.js',
    array( 'jquery' ),
    CHILD_THEME_VERSION,
    true
);
```

with

```
wp_enqueue_script(
    'genesis-sample',
    get_stylesheet_directory_uri() . "/js/genesis-sample{$suffix}.js",
    array( 'jquery' ),
    CHILD_THEME_VERSION,
    true
);
```

b) at the end, add

```
add_filter( 'stylesheet_uri', 'genesis_sample_stylesheet_uri', 10, 2 );
/**
 * Loads minified version of style.css.
 *
 * @param string $stylesheet_uri     Original stylesheet URI.
 * @param string $stylesheet_dir_uri Stylesheet directory.
 * @return string (Maybe modified) stylesheet URI.
 */
function genesis_sample_stylesheet_uri( $stylesheet_uri, $stylesheet_dir_uri ) {

	return trailingslashit( $stylesheet_dir_uri ) . 'style.min.css';

}
```

Note: You will not be able to use the source maps when style.min.css is loading. Therefore add the above code after you are done with your development.

## Credit

Thanks to Christoph Herr for his [Prometheus](https://github.com/christophherr/prometheus) theme.
