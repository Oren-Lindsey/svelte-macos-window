# svelte-macos-window
Draggable, macOS style windows for svelte

## Install
To install, run `npm i svelte-macos-window`

## Usage

```
<script>
    import Window from 'svelte-macos-window'
</script>
<Window x=0 y=0 gpuAcceleration=true title="demo" disabled=false><p>hello world</p></Window>
```

## Props
- `x`: starting x position. Type: `number`. Units: css pixels. Required: `true`
- `y`: starting y position. Type: `number`. Units: css pixels. Required: `true`
- `disabled`: whether it's draggable or not. Type: `boolean`. Required: `true`
- `gpuAcceleration`: whether it should be gpu accelerated (in some cases it leads to blurry text). Type: `boolean` Required: `true`
- `title`: The title of the window. Type: `string`. Required: `true`

