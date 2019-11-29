# NEXT.js: Getting Started

## Add the Dependencies

```bash
yarn add react react-dom next
```

## Set Up the Basics

- Make a folder in the root directory called pages. This is going to hold our site's pages.

- Update package.json to use the *next* commands:

```javascript
"scripts": {
  "dev": "next",
  "build": "next build",
  "start": "next start"
}
```

- Add pages to the pages folder. **index.js** will be our home page.

**index.js**

```javascript
import React from 'react';

function Index() {
  return (
    <div>
      <h1>This is Next js. Lol</h1>
    </div>
  )
}

export default Index
```

## Navigate Between Pages

1. Import Link from next/link:

```javascript
import Link from 'next/link'
```

2. Use Link:

```javascript
function Index() {
  return (
    <div>
      <Link href="/about">
        <a>About Page &rarr;</a>
      </Link>
      <h1>This is Next js. Lol</h1>
    </div>
  );
}
```

3. Adding Link Props

We may need to add attributes or props to our links for a number of reasons. Maybe we need to add a *title* attribute to the link:

```javascript
<Link href="/about">
  <a title="About Page">About Page</a>
</Link>
```

Once we do that, we can see that the rendered anchor tag get the *title* attribute "About Page" added on to it. 

But if we try to add the title directly to the Link like this:

```javascript
<Link href="/about" title="About Page">
  <a>About Page</a>
</Link>
```

The title will not be applied and we will get an error in the console: *Failed prop type: Link; unknown props found: title*

This is because Link is just a Higher Order Component.

The title prop on the *next/link* component has no effect. *next/link* only accepts the *href* and some similar props. If we need to add props to it, we need to do it to the underlying component. We can't expect the *next/link* component to pass those props to it's children.

In this case, the child of the *next/link* component is the anchor tag. It can also work with any other component or tag, the only requirement for components placed inside *<Link />* is that they should accept an *onClick* prop.

## Setting Up Emotion For Styling

1. Install the dependencies

```bash
yarn add @emotion/core @emotion/styled babel-plugin-emotion
```

2. Tell Babel that we want to use the emotion plugin:

**.babelrc**

```javascript
{
  "preset"
}
```

## Using a Layout Component

In our apps, sometimes we want a common style across various pages. For this purpose, we can create a common Layout component and use it for each of our pages:

```javascript
import Header from "./Header";

var layoutStyle = {
  margin: 20,
  padding: 20,
  border: "1px solid #DDD"
};

function Layout(props) {
  return (
    <div style={layoutStyle}>
      <Header />
      {props.children}
    </div>
  );
}
```

And then we can use the layout that we've created inside of our pages:

```javascript
import Layout from "../components/Layout";

function About() {
  return (
    <Layout>
      <h2>This is my about page...</h2>
    </Layout>
  );
}
```

## Alternate Methods For Setting Up a Layout Component

### Method 1 - Layout as a Higher Order Component

```javascript
import Header from "./Header"

const layoutStyle = {
  margin: 20,
  padding: 20,
  border: '1px solid #DDD'
}

const withLayout = Page => {
  return () => (
    <div style={layoutStyle}>
      <Header />
      <Page />
    </div>
  )
}

export default withLayout;
```

**pages/index.js**

```javascript
import withLayout from "../components/MyLayout";

function Page() {
  return (
    <p>Hello Next.js</p>
  )
}

export default withLayout(Page)
```

### Method 2 - Page Content As a Prop

```javascript
import Header from "./Header"

const layoutStyle = {
  margin: 20,
  padding: 20,
  border: '1px solid #DDD'
}

function Layout(props) {
  <div style={layoutStyle}>
    <Header />
    {props.content}
  </div>
}
```

**pages/index.js**

```javascript
import Layout from "../components/MyLayout"

var indexPageContent = <p>Hello Next.js</p>

export default function Index() {
  return <Layout content={indexPageContent} />
}
```