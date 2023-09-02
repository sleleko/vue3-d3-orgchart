# vue3-d3d-orgchart

This is Vue3 Org chart based on D3.js and Vue3 composition API.

## Screen
![Alt text](src/assets/img.png?raw=true "Screen example")

## Supported data format
```json lines
{
  name: 'CEO',
  contact: 'ceo@example.com',
  children: [
    {
      name: 'CTO',
      contact: 'cto@example.com',
      children: [
        { name: 'Engineer1', contact: 'eng1@example.com' },
        { name: 'Engineer2', contact: 'eng2@example.com' }
      ]
    },
    {
      name: 'CFO',
      contact: 'cfo@example.com'
    },
    {
      name: 'CMO',
      contact: 'cmo@example.com',
      children: [
        { name: 'Marketing1', contact: 'mkt1@example.com' }
      ]
    }
  ]
}
```

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```
