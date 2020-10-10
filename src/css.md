# CSS

[Convert image to svg for free](https://picsvg.com/)

## Design

https://uidesigndaily.com/

## Code Snippets

### Flexbox:

Move individual child to right:

> `margin-left: auto;`

### Full page scroll

```css
.container {
  max-height: 100vh;
  overflow: scroll;
  scroll-snap-type: y mandatory;
}

.children {
  scroll-snap-align: start;
}
```

### background-image url()

```css
.bg-img {
  /* gradient adds a layer of color on top of image */
  background: radial-gradient(#fff, #000), url('./bg.jpg') no-repeat center;
  background-size: cover;
  /* filter changes the image itself */
  filter: brightness(30%);
}
```

### scrollbar styling

```css
.content {
  overflow-y: scroll;
  scrollbar-width: thin;
  scrollbar-color: #90a4ae #cfd8dc;
}

.content::-webkit-scrollbar {
  width: 10px;
  height: 50px;
}

.content::-webkit-scrollbar-thumb {
  background: #90a4ae;
  border-radius: 5px;
}
.content::-webkit-scrollbar-track {
  background: #cfd8dc;
}
```

### neon color

```css
/* Text Color */
.blue-text {
  font-family: 'Vibur', 'Damion', cursive, sans-serif;
  color: rgb(211, 229, 255);
  text-shadow: 1px 5px 2px rgba(2, 0, 31, 0.712), 0 0 45px rgb(80, 83, 255),
    0 0 45px rgb(4, 0, 255), 0 0 4px rgb(192, 235, 255);
}

/* Border Color */
.blue-border {
  display: inline-block;
  padding: 10px 20px;
  background: rgba(0, 0, 255, 0.315);
  border: 5px solid rgb(211, 229, 255);
  border-radius: 10px;
  box-shadow: 4px 5px 2px rgba(2, 0, 31, 0.712), 0 0 70px rgb(0, 0, 255);
}
```

### Grid

```css
body {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr 1fr;
  grid-template-areas:
    'header header'
    'location map'
    'weather map';
  grid-row-gap: 1rem;
}

.grid-a {
  grid-area: header;
}
.grid-b {
  grid-area: location;
}
.grid-c {
  grid-area: map;
}
.grid-d {
  grid-area: weather;
}
```
