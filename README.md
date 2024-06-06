Hi there! 👋 I’m Adam from Wrocław, Poland – a WordPress core committer at Automattic and the creator of WordPress Playground.

Here's few noteworthy things I've worked on.

## WordPress Playground

WordPress Playground is WordPress in a single click. It runs directly in your web browser – there are no tedious setup steps, webhosts account, or technical talk. Just click, and it's there.

[<kbd> <br> Official Playground site on WordPress.org <br> </kbd>](https://wordpress.org/playground)

I [prototyped Playground in a week](https://adamadam.blog/2023/07/01/wordpress-playground-was-prototyped-in-a-week/) in 2022 and been leading the project since then. Initially I just wanted to write an [interactive tutorial](https://adamadam.blog/2023/02/16/how-to-modify-html-in-a-php-wordpress-plugin-using-the-new-tag-processor-api/) without maintaining a hosted WordPress, but Playground became much bigger than.

Playground is truly groundbreaking. It's a renaissance of a new generation of interactive, single-click WordPress tools. There are interactive tutorials, QA (Quality Assurance) workflows, “try before you buy” previewers for plugins, collaboration tools, contribution workflows and so much more. It's also an innovation incubator where we explore [Blueprints](https://github.com/WordPress/blueprints), [live synchronization](https://playground.wordpress.net/demos/sync.html), [native PHP XML parsers](https://github.com/WordPress/wordpress-develop/pull/6713), and more.

I wrote and spoke about Playground on a few occasions:

* https://web.dev/articles/wordpress-playground
* [WordCamp EU 2023](https://youtu.be/7Nmz3IjtPh0?si=f0lOhl8q1au-uyVP&t=371)
* [State of the Word 2022](https://youtu.be/VeigCZuxnfY?si=HuWxAykpddXzzO7l&t=2916)
* [State of the Word 2023](https://youtu.be/c7M4mBVgP3Y?si=inAaywBlrFyh1w1b&t=736)

Here's a few more resources:

* [WordPress Playground Demo](https://playground.wordpress.net/)
* [GitHub repository](https://github.com/WordPress/wordpress-playground/)

[<kbd> <br> Official Playground site on WordPress.org <br> </kbd>](https://wordpress.org/playground)

## Libraries

I've built many data processing libraries for PHP and TypeScript. I was surprised to learn there were no libraries for parsing, say, XML or HTML in PHP. Well, there were some, but I needed one the was small, optimized, had no dependencies, and would work for 100% of WordPress users. Well 🤷 Now we have those and more:

### PHP

- [WP_HTML_Tag_Processor](https://developer.wordpress.org/reference/classes/wp_html_tag_processor/)
- [WP_XML_Processor](https://github.com/WordPress/wordpress-develop/pull/6713) – Streaming XML parser (based on WP_HTML_Processor).
- [ZipStreamReader](https://github.com/WordPress/blueprints-library/blob/87afea1f9a244062a14aeff3949aae054bf74b70/src/WordPress/Zip/ZipStreamReader.php) and [ZipStreamWriter](https://href.li/?https://github.com/WordPress/blueprints-library/pull/103) – Streaming ZIP parser and encoder.
- [AsyncHttpClient](https://github.com/WordPress/blueprints-library/blob/trunk/src/WordPress/AsyncHttp/Client.php) – A streaming, non-blocking, concurrent, HTTPS-enabled, progress-monitoring HTTP client that works with barebones PHP 7.0 without any extensions.

All three could be piped together (with [minimal glue for WP_XML_Processor](https://href.li/?https://github.com/adamziel/wordpress-develop/pull/43)) using native PHP resource variables (`$fp`).

## WordPress

* [MySQL -> SQLite translator](https://github.com/WordPress/sqlite-database-integration) – to run WordPress on SQLite
* [WordPress -> ZIP exporter](https://github.com/WordPress/playground-tools/blob/974cb39df65089002a1bbf6f5eacd99a66d81801/packages/playground/src/playground-zip.php#L49) – streams all files and a database dump into ZIP archive and then directly to the response.

### TypeScript

- `@php-wasm/stream-compression` – [Streaming ZIP compression and decompression](https://href.li/?https://github.com/WordPress/wordpress-playground/pull/880)
    - [nextZipEntry()](https://github.com/WordPress/wordpress-playground/blob/726fc68309b0dcffc40402d7e0e7e68ed5fee01a/packages/playground/stream-compression/src/zip/parse-stream.ts#L88-L94) – extracts the next file from a zip archive
    - [encodeZip()](https://github.com/WordPress/wordpress-playground/blob/726fc68309b0dcffc40402d7e0e7e68ed5fee01a/packages/playground/stream-compression/src/zip/compress.ts#L34) – compresses File objects into a zip archive (stream of bytes)
    - [decodeRemoteZip()](https://github.com/WordPress/wordpress-playground/blob/726fc68309b0dcffc40402d7e0e7e68ed5fee01a/packages/playground/stream-compression/src/zip/parse-remote.ts#L86-L92) – lists ZIP files in a remote archive, filters them, and then downloads just the subset of bytes we need to get those files
- [TCP -> fetch() proxy](https://github.com/WordPress/wordpress-playground/pull/1093) – Stream-translates raw TCP bytes into a fetch() request and stream-encodes the response bytes. Supports HTTPS.
- [Inbound TCP -> WebSockets proxy](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/node/src/lib/networking/inbound-tcp-to-ws-proxy.ts) and [outbound WebSockets -> TCP proxy](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/node/src/lib/networking/outbound-ws-to-tcp-proxy.ts) – to connect a local WebAssembly Playground running in Node.js to the internet.

### C

- [PHP SAPI for WebAssembly](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/compile/php/php_wasm.c) – it takes request details from JavaScript and returns response bytes. It has basic support for piping streams to and from JavaScript. Works best with TypeScript [PHPRequestHandler](https://github.com/WordPress/wordpress-playground/blob/7d1653cc13cee59984572869071b1c5b64d318b4/packages/php-wasm/universal/src/lib/php-process-manager.ts) and [PHPProcessManager](https://github.com/WordPress/wordpress-playground/blob/7d1653cc13cee59984572869071b1c5b64d318b4/packages/php-wasm/universal/src/lib/php-process-manager.ts).

