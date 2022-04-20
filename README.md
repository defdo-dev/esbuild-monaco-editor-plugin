# esbuild-monaco-editor-plugin

This plugin tries to make esbuild work with monaco-editor.

> It is not a complete solution, but it is a start.
> Currently if you imports from `editor.api.js` you must import the languages to highlight.
> This is not a problem for the monaco-editor, it is a problem on my experience so feel free to PR.
> If you import from `editor.main.js` all the languages are loaded and the highlight works as expected.

```javascript
// esbuild.js
const monacoEditorPlugin = require("esbuild-monaco-editor-plugin");
const esbuild = require('esbuild');

// Decide which mode to proceed with
let mode = 'build';
process.argv.slice(2).forEach((arg) => {
  if (arg === '--wtach') {
    mode = 'watch';
  } else if (arg === '--deploy') {
    mode = 'deploy';
  }
});

ebuild.build({
  entryPoints: ['js/app.js'],
  bundle: true,
  outdir: '../priv/static/assets',
  plugins: [monacoEditorPlugin],
}).then((result) => {
  if (mode === 'watch') {
    process.stdin.pipe(process.stdout);
    process.stdin.on('end', () => { result.stop(); });
  }
}).catch((error) => {
  process.exit(1);
});
```