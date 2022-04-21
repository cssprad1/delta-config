# Writing contents for Delta dashboard

- [Writing contents for Delta dashboard](#writing-contents-for-delta-dashboard)
  - [Background & Prerequisites](#background--prerequisites)
  - [Block](#block)
  - [Image](#image)
    - [Inline image & Figure image](#inline-image--figure-image)
      - [How to use local image (assets)](#how-to-use-local-image-assets)
  - [Chart](#chart)
  - [Map](#map)
  - [Scrollytelling](#scrollytelling)
    - [Chapter properties](#chapter-properties)
  - [Some gotchas](#some-gotchas)

## Background & Prerequisites

Delta dashboard content uses [MDX](https://mdxjs.com/docs/what-is-mdx/) for its content. To most simply put, MDX combines Javascript components and Markdown. By using MDX, Delta dashboard can offer editors rich experience with custom components while still having a way of writing text-based content with markdown syntax.

Understanding of MDX is not required to write contents for Delta dashboard, but you need to know how to write [Markdown](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax), and to be familiar with the concept of [JSX](https://facebook.github.io/jsx/).

## Block

`Block` is a basic 'building block' for Delta dashboard contents. Any contents needs to be wrapped with `Block` component. The type of Block, and the combination of its children elements will decide the layout of the content block. When there is a layout change, you can assume that there is a change of block type. The image below shows what block was used for each layout.

<table>
<tr>
<td>

![How blocks look on discovery page](./media/prose-figure.jpg)   
 </td>
 <td > 
 
 ![blocks with each layout name labeled.](./media/prose-figure-w-quotation.jpg)   
  </td> 
</tr>
</table>


We currently (2022, March) have 8 types of Blocks for layout. Mind that only `Prose` and `Figure` can be direct children of Block. Any raw markdown contents can be wrapped with `Prose`. Any media contents or custom components (`Image`, `Map`, `Chart` ...) should be wrapped with `Figure`.

> If you are using a `Block` with more than one child element, mind that the order of children decides which one goes where. For example, in `FigureProse` Block, `<Figure>` comes before `<Prose>` in the syntax. In result, `Figure` shows up on the left, and `Prose` shows up on the right.

Layouts do work in any size of screen, but this documentation mainly addresses how they are represented on large (> 991px) screens.

<table style="margin-top: 20px">
<tr>
<th> Type </th><th width='300px'> Syntax </th> <th> Result </th>
</tr>
<tr>
  <td> Default Prose Block </td>
  <td> 

  ```jsx
  <Block>
    <Prose>
      ### Your markdown header

      Your markdown contents comes here.
    </Prose>
  </Block>
  ```  
  </td> 
  <td>  

  ![Screenshot of Default Prose Block](./media/delta-default-prose.jpg)   
  </td>
</tr>

<tr>
  <td> Wide Prose Block </td>
  <td> 

  ```jsx
  <Block type='wide'>
    <Prose>
      ### Your markdown header

      Your markdown contents comes here.
    </Prose>
  </Block>
  ```  
  </td> 
  <td>  

  ![Screenshot of Wide Prose Block](./media/delta-wide-prose.jpg)
  </td>
</tr>

<tr>
  <td> Wide Figure Block </td>
  <td> 

  ```jsx
  <Block type='wide'>
    <Figure>
      <Image ... />
      <Caption ...> caption </Caption>
    </Figure>
  </Block>
  ```

  </td> 
  <td>  

  ![Screenshot of Wide Figure Block](./media/delta-wide-figure.jpg)
  </td>
</tr>

<tr>
  <td> Full Figure Block </td>
  <td> 

  ```jsx
  <Block type='full'>
    <Figure>
      <Image ... />
      <Caption ...> caption </Caption>
    </Figure>
  </Block>
  ```
  </td> 
  <td>  

  ![Screenshot of Full Figure Block](./media/delta-full-figure.jpg)
  </td>
</tr>

<tr>
  <td> Prose Figure Block </td>
  <td> 

  ```jsx
  <Block>
    <Prose>
      My markdown contents
    </Prose>
    <Figure>
      <Image ... />
      <Caption> ... </Caption>
    </Figure>
  </Block>
  ```
  </td> 
  <td>  
  
  ![Screenshot of Prose Figure Block](./media/delta-prose-figure.jpg)
  </td>
</tr>


<tr>
  <td> Figure Prose Block </td>
  <td> 

  ```jsx
  <Block>
    <Figure>
      <Image ... />
      <Caption> ... </Caption>
    </Figure>
    <Prose>
      My markdown contents
    </Prose>
  </Block>
  ```
  </td> 
  <td>  

  ![Screenshot of Figure Prose Block](./media/delta-figure-prose.jpg)
  </td>
</tr>

<tr>
  <td> Prose Full Figure Block </td>
  <td> 

  ```jsx
  <Block type='full'>
    <Prose>
      My markdown contents
    </Prose>
    <Figure>
      <Image ... />
      <Caption> ... </Caption>
    </Figure>
  </Block>
  ```
  </td> 
  <td>  

  ![Screenshot of prose full figure Block](./media/delta-prose-full-figure.jpg)
  </td>
</tr>

<tr>
  <td> Full Figure Prose Block </td>
  <td> 


  ```jsx
  <Block type='full'>
    <Figure>
      <Image ... />
      <Caption> ... </Caption>
    </Figure>
    <Prose>
      My markdown contents
    </Prose>
  </Block>
  ```
  </td> 
  <td>  

  ![Screenshot of full figure prose Block](./media/delta-full-figure-prose.jpg)
  </td>
</tr>
</table>


## Image 

To offer rich visual and better experience, Delta dashboard offers `Image` component, which is a wrapper for `<img/>` HTML tag. You can use `Image` component to display any kind of image. Depending on where Image is used (is it inside of `Prose` as an inline image? or inside of `Figure`?), there are additional attributes you need to pass.

Also you can pass any attribute that you can use with `<img />` HTML element and these will get passed down. Ex. you can pass width of image or height of image with `width`, `height`.

| Option | Type | Default | Description|
|---|---|---|---|
| src | string | `''` | Path for image. If using local image, please look at the section below. |
| alt | string | `''` | Description for image, this will be used for screen readers. |
| align | string, enum (left, right, center) | `center` | <b>For inline image.</b> Alignment of image. |
| caption | string | `''` | <b>For inline image.</b>  Caption text for inline image. |
| attrAuthor | string | `''` | Info for image author. When omitted, attribution mark on the right-top part of the figure wouldn't show up. |
| attrUrl | string | `''` | Link for image attribution. |

### Inline image & Figure image

`Image` component can take different attributes depending on its context.  

When `Image` is used in `Prose`, it is inline image ad should be used when you need to put an image inside of `Prose`.

<table>
  <tr>
    <th>Syntax</th>
    <th>How it looks on the page</th>
  </tr>

  <tr>
  <td>

  ```jsx
    <Image 
      src="http://via.placeholder.com/256x128?text=align-left" 
      alt="Media example" 
      align="left" 
      caption="example caption" 
      attrAuthor="example author"
      attrUrl="https://example.com"
      width="256"
    />
  ```
  </td>
  <td> 
 
  ![Screenshot of inline Image component](./media/image-inline.jpg)
  </td>
  </tr>
</table>


When `Image` is used in `Figure`, it is Figure image.  
You can replace `attr` option with `<Caption>` component if your image is used in `Figure` block. In this way, you can display rich text as Caption. 

<table>
  <tr>
    <th>Syntax</th>
    <th>How it looks on the page</th>
  </tr>

  <tr>
  <td>

  ```jsx
    <Figure type='full'>
      <Image
        src="http://via.placeholder.com/1200x800?text=figure" 
        alt='description for image'
      />
      <Caption 
        attrAuthor='Development Seed' 
        attrUrl='https://developmentseed.org'
      >
        This is an image. This is <a href="link">a link</a>.
      </Caption> 
    </Figure>
  ```
  </td>
  <td> 
 
  ![Screenshot of full figure Image component](./media/image-figure.jpg)
  </td>
  </tr>
</table>



#### How to use local image (assets)

Because of internal build process, you need to wrap the path with specific template when using local assets like below.

```js
new URL('where-your-image-is.jpg', import.meta.url).href
```

For example, if you put an image `image.jpg` inside of the folder where your mdx file is, the syntax for `Image` component will be like below.

```jsx
<Image
  src={new URL('./img.jpg', import.meta.url).href}
  align="left" 
  attr="tux" 
  attrAuthor="penguin"
  attrUrl="https://linux.org"
  width="256" 
/>
```

## Chart

| Option | Type | Default | Description|
|---|---|---|---|
| dataPath | string | `''` | Path for data. The data should be either in `csv` or `json` format. |
| xKey | string | `''` | Attribute to be used for x axis. |
| yKey | string | `''` | Attribute to be used for y axis |
| idKey | string | `''` | Attribute for each data point |
| dateFormat | string | `''` | Template for how temporal date is formatted. This follows [d3's convention for date format](https://github.com/d3/d3-time-format#locale_format) |
| highlightStart | string | `''` | Start point for x axis to draw highlighted area. |
| highlightEnd | string | `''` | End point of x axis to draw highlighted area.
| highlightLabel | string | `''` | Label for highlighted area. |

Syntax for Chart used in Wide Figure Block looks like this

```jsx
<Block type='wide'>
  <Figure>
    <Chart
      dataPath='example.csv'
      dateFormat="%m/%d/%Y" 
      idKey='County' 
      xKey='Test Date' 
      yKey='New Positives' 
      highlightStart = '12/10/2021'
      highlightEnd = '01/20/2022'
      highlightLabel = 'Omicron'
    />
    <Caption 
      attrAuthor='attribution for wide figure block, chart' 
      attrUrl='https://developmentseed.org'
    /> 
  </Figure>
</Block>
```

## Map

| Option | Type | Default | Description|
|---|---|---|---|
| datasetId | string | `''` | `id` defined in dataset mdx. |
| layerId | string | `''` | `id` for layer to display. The layer should be a part of the dataset above. |
| dateTime | string | `''` | Optional. This string should follow `yyyy-mm-dd` format. When omitted, the very first available dateTime for the dataset will be displayed |
| isComparing | boolean | `false` | Optional. If the compare layer in the dataset needs to be turned on, pass `true`. |

Syntax for Map, which displays `nightlights-hd-monthly` layer from `sandbox` dataset in full figure block looks like this:

```jsx
<Block type='full'>
  <Figure>
    <Map datasetId='sandbox' layerId='nightlights-hd-monthly' dateTime='2020-03-01' isComparing={false} />
    <Caption>
      The caption displays below the map.
    </Caption>
  </Figure>
</Block>
```

## Scrollytelling

> "Scrollytelling" was a term first coined to describe online longform stories characterised by audio, video and animation effects triggered by simply scrolling the page. - [An introduction to scrollytelling](https://shorthand.com/the-craft/an-introduction-to-scrollytelling/index.html).

![](./media/scrollytelling.png)

The Scrollytelling feature of Delta is map based and allows you to define different `Chapters` where each chapter corresponds to a map position and layer being displayed.  
As the user scrolls the chapter content comes into view on top of the map which will animate to a specific position.

The scrollytelling is defined as a series os `Chapters` inside the `ScrollytellingBlock`.

```jsx
<ScrollytellingBlock>
  <Chapter
    center={[0, 0]}
    zoom={2}
    datasetId='no2'
    layerId='no2-monthly-diff'
    datetime='2021-03-01'
  >
    ## Content of chapter 1

    Markdown is supported
  </Chapter>
  <Chapter
    center={[-30, 30]}
    zoom={4}
    datasetId='no2'
    layerId='no2-monthly-diff'
    datetime='2020-03-01'
  >

  Each chapter is a box where content appears.
  </Chapter>
</ScrollytellingBlock>
```

### Chapter properties
| Option | Type | Description |
|---|---|---|
| center | [number, number] | Center coordinates for the map [Longitude, Latitude] |
| zoom | number | Zoom value for the map |
| datasetId | string | `id` of the Dataset to which the layer to to display belongs |
| layerId | boolean | `id` of the dataset layer to display |
| datetime | boolean | Optional. If the layer to display has a temporal extent, specify the datetime |

## Some gotchas

- Do not use h1(`# heading 1`) for your header. `h1` is reserved for page title.