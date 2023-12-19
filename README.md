# Svelte Time Picker

## What is it?

A very simple time picker component for Svelte, inspired by Material UI's time picker.

## Installation

```bash
npm install @rschiang/svelte-timepicker
```

## Usage

```svelte
<script>
    import { TimePicker } from "@rschiang/svelte-timepicker";

    let hours = null;
    let minutes = null;
</script>

<TimePicker onChange={(h, m) => {
    hours = h;
    minutes = m;
}} />
```

## Demo

[https://svelte.dev/repl/54e378a10dac4c8cabd9d938d40a9364?version=4.2.8](https://svelte.dev/repl/54e378a10dac4c8cabd9d938d40a9364?version=4.2.8)

![Screenshot](./static/screenshot.png)

## Properties

| Property | Type | Description |
| ----------- | ----------- | ----------- |
| hours? | `number \| null` | Controls the hours value. |
| minutes? | `number \| null` | Controls the minutes value. | 
| onChange? | `function \| null` | Function with two parameters: `hours` and `minutes`. |