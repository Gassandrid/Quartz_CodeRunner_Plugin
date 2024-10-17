# Python Code Runner For Quartz

This plugin searches for all codeblocks in your notes, finds the python ones, and includes a script to run the code and display the results.

This is primarily designed for visualization in mind, and works well with matplotlib and numpy.

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

### Extra but not necessary

- add the custom scss for your quartz
  - some of the css is inline within the plugin, if you want to change it

```css
[saved-theme="dark"] .python-output {
  background-color: var(--shiki-dark-bg);
  color: var(--shiki-dark);
}

[saved-theme="light"] .python-output {
  background-color: var(--shiki-light-bg);
  color: var(--shiki-light);
}

.python-output {
  border-radius: 5px;
}
```

## Credits

Huge thanks to **saberzero1** in the Quartz Discord for his help and patience in debugging the very wierd errors that arose on this project.
