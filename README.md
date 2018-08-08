<div>
  <!-- npm -->
  <a href="https://www.npmjs.com/package/@honkjs/components">
    <img src="https://img.shields.io/npm/v/@honkjs/components.svg?style=flat-square" alt="npm version" />
  </a>
  <!--  dependencies -->
  <a href="https://david-dm.org/honkjs/components">
    <img src="https://david-dm.org/honkjs/components.svg?style=flat-square" alt="dependency status" />
  </a>
  <!-- dev dependencies  -->
  <a href="https://david-dm.org/honkjs/components&type=dev">
    <img src="https://david-dm.org/honkjs/components/dev-status.svg?style=flat-square" alt="dev dependency status" />
  </a>
  <!-- coverage -->
  <a href="https://codecov.io/github/honkjs/components">
    <img src="https://img.shields.io/codecov/c/github/honkjs/components/master.svg?style=flat-square" alt="test coverage" />
  </a>
  <!-- build -->
  <a href="https://travis-ci.org/honkjs/components">
    <img src="https://img.shields.io/travis/honkjs/components/master.svg?style=flat-square" alt="build status" />
  </a>
</div>

# honkjs/components

A honk middleware for creating and caching choo [nanocomponents](https://github.com/choojs/nanocomponent).

This is probably the most opinionated of the basic honk middlewares.

# Basics

First, the component:

```ts
import { IHonkServices } from '@honkjs/honk';
import { createHonkComponent } from '@honkjs/components';
import html from 'choo/html';
import Component from 'choo/component';

type HelloComponentProps = { id: string; name: string };

class HelloComponent extends Component {
  private prev?: string;

  createElement({ name }: HelloComponentProps) {
    this.prev = name;
    return html`<div>Hello, ${name}</div>`;
  }

  update({ name }: HelloComponentProps) {
    return this.prev !== name;
  }
}

const create = createHonkComponent('Hello', (services: IHonkServices, id: string, props: HelloComponentProps) => {
  return new HelloComponent();
});

export default create;
```

And how we use it:

```ts
import Honk from '@honkjs/honk';
import components from '@honkjs/components';
import hello from './HelloComponent';

const honk = new Honk().use(components()).honk;

honk(create, { id: 'yo', name: 'bob' });
// returns:  html`<div>Hello, bob</div>`
```

The component itself is defined like normal. Honk only requires all the props to be passed as a single object.

To make a component instantiable via honk, it has to be wrapped by `createHonkComponent`. This creates a function that can be called to create or uncache the component as appropriate.

```ts
/**
 * The first argument is the name of the component.
 * This is prefixed to the id when when working to the cache.
 *
 * The second argument is the function to create a new instance of the component.
 * It's passed the honk services, the id (with prefix), and the initializing props.
 */
const create = createHonkComponent('Hello', (services: IHonkServices, id: string, props: HelloComponentProps) => {
  return new HelloComponent();
});
```

## ID from props

By default, the props must contain an "id" field used to locate the component. Alternatively, you can define a function to map the id from the passed property object.

For example:

```ts
function getIdFromProps(item: Item) {
  return item.id;
}

function createComponent(services: IHonkServices, id: string, props: Item) {
  return new ItemComponent();
}

const create = createHonkComponent('ItemView', createComponent, getIdFromProps);
```

# Using with choo

[still in transit 🚚]
