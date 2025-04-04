# Python Code Runner For Quartz

**RECENTLY OVERHAULED**: Much faster, asynchronous, and shared workspace for vars and functions

This plugin searches for all codeblocks in your notes, finds the python ones, and includes a script to run the code and display the results.

This is primarily designed for visualization in mind, and works well with matplotlib and numpy.

> [!WARNING] This is terrible code!!!!
> All the css is inline because I am lazy and was encountering some issues
> same goes for the javascript, where I had a wierd issue of " and < being encoded into their html entities `&quot;` and `&lt;` respectively

I could make the code runner faster, but at the cost of crashing the site if you run the codeblock before the resources are done loading, which takes around 10 seconds.

## Update

I have added a sort of IDE to go along with the code runner, allows you to edit the code as you go, collapse it for easy viewing, so on and so forth. This plugin might be getting a better expansion for more languages and so on.

## Installation

- Copy the `RunPythonPlugin.ts` into your Quartz under `quartz/plugins/transformers`

- update your transoformer's index.ts file to include the following line:

```typescript
export { RunPythonPlugin } from "./RunPythonPlugin";
```

- update your `quartz.config.ts` to include the following under plugins: transformers:

```typescript
Plugin.RunPythonPlugin(),
```

### Custom.css

I havent really overhauled this, but some css bugs remain so you will probably want to put this in your custom.css file. It is very verbose, so plesae play around/fix it if you want.

```css
.spinner {
  display: none;
  width: 20px;
  height: 20px;
  border: 2px solid #f3f3f3;
  border-top: 2px solid #3498db;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}
@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
.play-icon {
  width: 16px;
  height: 16px;
  vertical-align: middle;
}
.python-output {
  display: none;
  margin-top: 10px;
  padding: 10px;
  background-color: rgba(128, 128, 128, 0.5); /* grey and mostly transparent */
  border: 2px solid rgba(64, 64, 64, 0.8); /* darker gray and less transparent border */
  border-radius: 0.5rem !important;
}
.python-output.visible {
  display: block; /* make visible after running */
  border-radius: 0.5rem !important;
}

.code-block {
  width: 100%;
  border-radius: 0.5rem;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

[saved-theme="light"] .code-block {
  background-color: #eff1f5;
}

[saved-theme="dark"] .code-block {
  background-color: #1e1e2e;
}

.code-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem 1rem;
}

[saved-theme="light"] .code-header {
  background-color: #dce0e8;
  border-bottom: 1px solid #bcc0cc;
}

[saved-theme="dark"] .code-header {
  background-color: #313244;
  border-bottom: 1px solid #45475a;
}

.code-language {
  font-family: monospace;
  font-size: 0.875rem;
}

[saved-theme="light"] .code-language {
  color: #4c4f69;
}

[saved-theme="dark"] .code-language {
  color: #cdd6f4;
}

.code-actions {
  display: flex;
  gap: 0.5rem;
}

.code-actions button {
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.25rem;
  transition: color 0.2s;
}

[saved-theme="light"] .code-actions button {
  color: #8839ef;
}

[saved-theme="dark"] .code-actions button {
  color: #cba6f7;
}

[saved-theme="light"] .code-actions button:hover {
  color: #7287fd;
}

[saved-theme="dark"] .code-actions button:hover {
  color: #f5c2e7;
}

.code-content {
  position: relative;
  height: 150px;
  transition: height 0.3s ease-in-out;
  overflow: hidden;
}

.code-content.expanded {
  height: auto;
  max-height: none;
}

.CodeMirror {
  height: 100%;
}

.code-gradient {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 2rem;
  pointer-events: none;
}

[saved-theme="light"] .code-gradient {
  background: linear-gradient(
    to top,
    rgba(239, 241, 245, 1),
    rgba(239, 241, 245, 0)
  );
}

[saved-theme="dark"] .code-gradient {
  background: linear-gradient(to top, rgba(30, 30, 46, 1), rgba(30, 30, 46, 0));
}

.output-wrapper {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.3s ease-in-out;
  border-radius: 0.5rem;
  padding-top: 0.5rem;
}

.output-wrapper.expanded {
  min-height: 300px;
  max-height: none;
  border-radius: 0.5rem;
}

.output-content {
  font-family: monospace;
  font-size: 0.875rem;
  padding: 0.5rem 1rem;
  white-space: pre-wrap;
  word-break: break-all;
  overflow-y: auto;
}

[saved-theme="light"] .output-content {
  background-color: #dce0e8;
  color: #4c4f69;
}

[saved-theme="dark"] .output-content {
  background-color: #313244;
  color: #cdd6f4;
}

.output-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem 1rem;
}

[saved-theme="light"] .output-header {
  background-color: #dce0e8;
  border-top: 1px solid #bcc0cc;
}

[saved-theme="dark"] .output-header {
  background-color: #313244;
  border-top: 1px solid #45475a;
}

.output-title {
  font-family: monospace;
  font-size: 0.875rem;
}

[saved-theme="light"] .output-title {
  color: #4c4f69;
}

[saved-theme="dark"] .output-title {
  color: #cdd6f4;
}

.close-output-btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.25rem;
  transition: color 0.2s;
}

[saved-theme="light"] .close-output-btn {
  color: #8839ef;
}

[saved-theme="dark"] .close-output-btn {
  color: #cba6f7;
}

[saved-theme="light"] .close-output-btn:hover {
  color: #7287fd;
}

[saved-theme="dark"] .close-output-btn:hover {
  color: #f5c2e7;
}

// main center border with light/dark mode
// square
[saved-theme="dark"] .center {
  border: 1px solid var(--darkgray);
}

[saved-theme="light"] .center {
  border: 1px solid var(--darkgray);
}
```

## Credits

Huge thanks to **saberzero1** in the Quartz Discord for his help and patience in debugging the very wierd errors that arose on this project.
