# Svelte Time Picker

## What is it?

A simple time picker component for Svelte, inspired by Material UI's time picker.

## Installation

```bash
npm install @rschiang/svelte-timepicker
```

## Usage

```svelte
<script lang="ts">
    import { TimePicker } from "@rschiang/svelte-timepicker";

    let hours: number | null = null;
    let minutes: number | null = null;
</script>

<TimePicker onChange={(h, m) => {
    hours = h;
    minutes = m;
}} />
```
