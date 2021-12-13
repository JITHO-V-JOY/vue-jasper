# Sample of using visualize.js to load resources into Vue.js application

This project describes the sample of how the [Visualize.JS](https://community.jaspersoft.com/project/visualizejs) library can be integrated into Vue.js application.

## Quick overview
To launch this demo open file **src/components/Visualize.vue** and edit object **jrsConfig**: you need to specify credentials and URL to JRS instance.
Then, run next commands:
```
yarn install
yarn serve
```
or, if you prefer npm:
```
npm install
npm serve
```

## More details

Integration of Visualize.JS library made through creating a vue.js component which accepts two properties:
1. resourceType - one of the: adhoc, report, dashboard
2. resourcePath - path to the resource inside JRS

Then, components loads the Visualize.js library, initializes it with credentials and JRS path,
and runs the specified resource type.

The component uses hooks:
1. **created** to start process of loading visualize.js library
2. **mounted** to start rendering the resource from JRS

Also, the component takes 100% of width and height of the container, so it's better to control
the size of the rendered resource by its container.

### Sample usage:

First of all, you need to import Visualize component:
```
import Visualize from './components/Visualize.vue'
```
then, you need to register this component in Vue.js subsystem. For this it's better
to refer to Vue.js documents cause it depends ob where you want to use this component.
In a simple case it can be something like this:
```
  export default {
    name: 'App',
    components: {
      Visualize
    }
  }
```
then you may render it:
```
<Visualize
    resourceType="adhoc"
    resourcePath="/public/Samples/Ad_Hoc_Views/09__Store_Segment_Performance"
/>
```
which would render the resource in-place.
More complete example can be like this:
```
<div class="containerClass">
    <div class="title">Resource title</div>
    <Visualize
        resourceType="adhoc"
        resourcePath="/public/Samples/Ad_Hoc_Views/09__Store_Segment_Performance"
    />
</div>
```

### How it works

#### Loading of Visualize.js library

The loading is done by appending script visualize.js into body DOM node.

BWT, here is the list of some parameters to this script you may want to use:
1. _opt - boolean, enables use of optimized source code
2. userTimezone - timezone name, i.e. America/Los_Angeles
3. userLocale - locale name which is supported by JRS, some examples are: en, de, it

so, in this case the full URL would be:
**client/visualize.js?_opt=false&userTimezone=America/Los_Angeles&userLocale=en**
open the file **src/components/Visualize.vue**, find the "client/visualize.js", and add any parameters there.

Note: Visualize.js library injects itself into window object, like jQuery, and you can access it by
```
window.visualize
```

#### Creating a container

Visualize.js renders resources into containers, and it would be OK unless visualize.js currently accepts
a container's reference as a selector.
So, we need to tag the container of Visualize component with some id or name, or something.
This is what we do in code:
```
  const containerName = 'container' + containerId;
  this.$el.setAttribute('name', containerName);
  containerId++;
```
which later we pass to visualize.js library:
```
    container: `[name=${containerName}]`
```

This is going to be changed and support of passing DOM nodes and jQuery elements will be provided soon.

#### Passing custom options to visualize.js library
[Visualize.js](https://community.jaspersoft.com/project/visualizejs) library supports many options
which you may find on this page:

https://community.jaspersoft.com/documentation/tibco-jasperreports-server-visualizejs-guide/v62/api-reference-visualizejs

Also, here you can find some other tutorials about using Visualize.js:

https://community.jaspersoft.com/wiki/visualizejs-tutorials