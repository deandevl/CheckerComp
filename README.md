## checker-comp

**checker-comp** is a web component (Vue.js >= 2.5) that provides multiple checkboxes of items with a check image.  The programmer can control the mode of the checking where one checkbox is checked  and the previous check is unchecked; or multiple mode where any number of checkboxes can be checked.  

**checker-comp** depends on the [vue.js](https://vuejs.org/ "Vue.js") framework and can be installed via [npm install](https://docs.npmjs.com/cli/install.html "npm install") with the included `package.json` file.  Three [webpack](https://webpack.js.org/concepts/) npm scripts are included for building  development, production, or hot recompile/execute of the demo.   `build-dev` and `build-prod` scripts produce  a `dist` folder containing the `index.html`.  The size of the `main.js` bundle using `build-prod` is 14 KiB along with calling a CDN for incorporating the Vue framework.

## Props

A prop in Vue.js is a custom attribute for passing information from a parent component hosting **checker-comp** instance(s) to a **checker-comp** as a child component. 

**checker-comp** has the following props for a parent to bind to child:

  `heading` -- a heading to be displayed above the checkbox list (string,default: null)

  `items` -- the items that are to be checked or unchecked (array of objects -- see below for format, default null)

  `single_check` -- selection mode is one check at a time (boolean, default: false)

  `drop_panel_height`-- the height of drop down list panel (string, default: '100px')

  `blur_panel` -- if false, will not roll up item panel when component is out of focus (boolean, default: true)

  `css_variables` -- defines the css variables for **checker-comp** (object, default: null)

The `items`  property is an array of objects where each object has a `text` and `checked` property key as in the following example:

```
      items_dow: [
        {text: 'Sunday',checked: false},
        {text: 'Monday',checked: false},
        {text: 'Tuesday',checked: false},
        {text: 'Wednesday',checked: true},
        {text: 'Thursday',checked: false},
        {text: 'Friday',checked: false},
        {text: 'Saturday',checked: false}
      ]
```



## Styling

The **css_variables** prop is a javascript object that contains any combination of css variable names as keys and associated values.  The following list are the css variable names along with their default values for a quick styling of **checker-comp**:

```
  {
        checker_comp_font_family: 'Verdana,serif',

        checker_comp_heading_color: 'black',
        checker_comp_heading_font_size: '1rem',
        checker_comp_heading_font_weight: 'bold',
        
        checker_comp_down_icon: '\21D3';
        checker_comp_icon_color: 'black',

	    checker_comp_items_panel_position: 'static',
        checker_comp_items_panel_z_index: 'auto',
        checker_comp_items_panel_color: 'black',
        checker_comp_items_panel_background: 'white',
        checker_comp_items_panel_border: '1px solid black',

        checker_comp_checkbox_border: '1px solid black',
        checker_comp_notchecked_background: 'white',
        checker_comp_checked_background: 'yellowgreen',

        checker_comp_item_font_size: '.8rem'
  }
```

As an example you could bind from the parent the following object to the `css_variables` prop for setting the heading font size and the panel background of **checker-comp** :

```
{checker_comp_heading_font_size: '24px', checker_comp_items_panel_background: 'lightblue'}
```

Note that multiple **checker-comp**  children of the parent can each be bound to a unique set of `css_variable` prop objects. To enforce the same styling across all **checker-comp**  children, simply  bind just one `css_variable` prop object to all the **checker-comp**  children.

## Events

**checker-comp** has a single event that emits/notifies the parent component of the current checked items indices:

```
'checker_comp_checked_indices' -- returns indices of current checked items
```

The parent component can listen to the above event and know a **checker-comp**'s current checked indices for further processing.  The demonstration section below illustrates how a parent can respond to this event.  Events emitted from a child component back to the parent is explained at [Vue Custom Events](https://vuejs.org/v2/guide/components.html#Using-v-on-with-Custom-Events).

## Demonstration

One demonstration of **checker-comp** is provided in the folder named `demo` and can be viewed by hosting the `index.html`file under the `dist` folder.  The demo (templated in the `App.vue` file)  displays three independent **checker-comp** components along with buttons which when clicked will check additional items.  In addition all three component parents listen to the `checker_comp_checked_indices` event and console.logs the current checked indices.

As a suggestion, install [http-server](https://www.npmjs.com/package/http-server "http-server") globally via [npm](https://www.npmjs.com/ "npm") then enter the command `http-server`in the **checker-comp** `dist` directory.  From a browser enter the url: `localhost:8080/` to view the demo.

Here is some example code for using **checker-comp** taken from the demo:

```
        <checker-comp
          heading="Days of the Week"
          drop_panel_height="160px"
          :blur_panel="blur_panel"
          :items="items_dow"
          :css_variables="css_variables_days_of_week"
          v-on:checker_comp_checked_indices="dow_changed">
        </checker-comp>
```

And the supporting data references:

```
 data() {
    return {
      blur_panel: false,
      items_dow: [
        {text: 'Sunday',checked: false},
        {text: 'Monday',checked: false},
        {text: 'Tuesday',checked: false},
        {text: 'Wednesday',checked: true},
        {text: 'Thursday',checked: false},
        {text: 'Friday',checked: false},
        {text: 'Saturday',checked: false}
      ],
      ...
      ...
      ...
      css_variables: {
        checker_comp_items_panel_position: 'absolute',
        checker_comp_items_panel_z_index: '100'
      }
   }
 } 
 methods: {
 	//method that responds to 'checker_comp_checked_indices' event
    dow_changed: function(indices){
      console.log(`Days of Week Indices: ${indices}`)
    },
    ...
    ...
    ...
  }
```

