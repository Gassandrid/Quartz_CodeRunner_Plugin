# Python Code Runner For Quartz

A simple, asynchronous Python code runner plugin for Quartz. This plugin allows you to run Python code blocks directly within your Quartz notes, providing a convenient way to execute and view the output of your Python scripts.

## Demo

Can be seen on [my website](https://ewan.my/Notes/Computational-Neuroscience/Theory/Theoretical-Neuroscience---Peter-Dayan-and-L.-F.-Abbott/CH1---Firing-Rates-and-Spike-Statistics/Firing-Rate-Approximation), using the added scss styles to make the output look nice.

## Installation

1. Copy the `RunPythonPlugin.ts` into your Quartz under `quartz/plugins/transformers`

2. update your transformers' index.ts file to include the following line:

```typescript
export { RunPythonPlugin } from "./RunPythonPlugin";
```

3. update your `quartz.config.ts` to include the following under plugins: transformers:

```typescript
Plugin.RunPythonPlugin(),
```

4. Copy the `python-plugin.css` file into your Quartz under `quartz/styles`

5. Update your `custom.scss` file to include the following line:

```scss
@use "./python-plugin.scss";
```

## Credits

Huge thanks to **saberzero1** in the Quartz Discord for his help and patience in debugging the very wierd errors that arose on this project.
