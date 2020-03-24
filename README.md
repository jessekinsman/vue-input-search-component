# Vue-Input-Search
Simple view component for displaying a list of results that you want a user to select from. User can click into the input and start typing to see only results tha contain whatever they type. 
This is a great component if you have a several of items the user needs to select from and they might have an idea of what the item is called.


## Add input to your project withing a Vue template
```
<InputSearch v-bind:list="items" id="autocomplete" name="inputSearch" placeholder="Search Items" v-bind:checktop=true @selected="selected" />
```
### List data to pass to the input-search element
Data must be formed in the following way:
```
[
{
  name: "test 1",
  id: 1,
},
{
  name: "test 2",
  id: 2
},
{
  name: "test 3",
  id: 3
}
]
```
It can have other properties but it must be an array of objects that contain the name prop and the id prop. 

## Checktop Property (boolean)
The checktop prop, if passed as true will calculate the size of the list and determine if it should be displayed below or above the input element depending on proximity to the window (at bottom of window or top of window).

## Selection Event
When the user selects and item, an event is emitted from the component with the item that was selected as it's payload. 

So you want to listen for the @selected event
```
@selected="methodToCall"
```
The method that is called will be passed the item the user selected

## Working with the source files
This is a dead-simple component so it should be pretty easy to step in and cusomize the source anyway you see fit. 
Feel free to clone and customize. 

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Run your tests
```
npm run test
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
