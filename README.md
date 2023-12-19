# Svelte Time Picker

## What is it?

A simple time picker component for Svelte, inspired by Material UI's time picker.

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
