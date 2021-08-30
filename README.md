# This a Svelte + Vite app create using Vite Svelte template. You can initialize it with :

npx or pnpx create vite _name_of_your_project_ --template svelte

## The takeaways from the chapters of the [MDN Svelte Tutorial][mdn-svelte]

1.  Provide props to the App.svelte component:

    In the main.js file imported by the index.html, initialize the App component with a `prop` property :

    ```js
    import App from "./App.svelte";

    const app = new App({
      target: document.getElementById("app"),
      props: {
        name: "world",
      },
    });

    export default app;
    ```

2.  a11y : Declare that a button has been pressed

    To declare that a button has been pressed, which is useful if the state of the app/page is a result of this button action, we use the aria attribute `aria-pressed`:

    ```html
    <button aria-pressed="true">See All elements</button>
    ```

3.  a11y : Hide text from sighted users but useful to screen readers

    Screen readers read the content in our html to provide the info vocally. So we can have more text to provide than we want or have place to display them.

    Wrap text providing longer version of content in `span` elements then hide them with class.
    This hiding strategy is interesting to apply on buttons or control elements where we usually only provide short `label` elements to describe their function:

    ```html
    <button>
      <span class="visually-hidden">Show</span>
      <span>All</span>
      <span class="visually-hidden">tasks</span>
    </button>
    ```

4.  a11y: Declare a role to `ul` and `ol` elements

    Even if `ul` and `ol` elements have by default a function of list, visual styles may break that functionality.

    Declare of role of `list` on restore the semantic meaning of these elements

    ```html
    <ul role="list">
      ...
    </ul>
    ```

5.  a11y: Declare a text element as label of another

    ```html
    <h1 id="list-heading">...</h1>

    <ul ... aria-labelledby="list-heading">
      ...
    </ul>
    ```

    the `ul` element tells screen readers that it is labelled by the heading element with the id "list-heading". The content of that heading is then used as a description of the list element.

6.  global.css : in Vite projects, import from assets folder

    Place the file in the assets folder at the root of the project then add the file in the App.svelte with :

    ```html
    <script>
      import "./assets/global.css";
      ...
    </script>
    ```

7.  svelte : use of javascript in element attributes

    With `todo` provided by an {#each} directive, js variables and expressions can be used in attributes expressions without the `$` sign.

    ```html
    <input type="checkbox" id="todo-{todo.id}" checked="{todo.completed}" />
    <label for="todo-{todo.id}" class="todo-label"></label>
    ```

8.  svelte : use of function for element event handler

    Suppose we have defined a function to use as an element event handler.

    - If the handler does not use any params, call the function like this:

      `on:event={handleEvent}`

    - If the handle needs params, call the function like this :

      `on:event={() => handleEvent(param)}`

      **Error - Do not call the handler using params like below :**

      `on:event={handleEvent(param)}`

      This is will call the function with the params then pass the result as a handler.

9.  svelte : conditionally apply a class on an element

    `<div class:classname={set_classname}`

    `set_classname` is a expression that evaluates to a boolean value

10. svelte : reactivity

    If template elements depends on a function, all variables changing the result of that function should be explicity featured in the template expression, otherwise Svelte is not able to react to the change of that variable and recalculate the updated list of elements.

    Suppose we have a function f :

    ```
    f() {
      ... variable v1 is used to calculate the result
      ... but not called by a parameter but directly accessed inside the function
    }
    ```

    in the template expression :

    ```
    {#each f()}

    {/each}
    ```

    the each block will not be recalculated if v1 changes

    To have the #each block recalculate on change of v1, the function should explicitely use v1 as a parameter then used inside the #each block as:

    ```
    {#each f(v1)}

    {/each}
    ```

11. svelte : Two-way binding to send data from child to parent

    To pass data from the child back up to its parent, we can bind a child variable to a parent variable via the `bind:` directive :

    `<MyComponent bind:childvar={parentVar}``

    childvar is a variable declared in svelte component MyComponent
    parentVar is a variable declared in the component calling My Component

12. svelte : dispatch event to communicate to parent component

    A child component can send events with an object carrying data to its parent

    In the child component:

    ```html
    <script>
      import { createEventDispatcher } from 'svelte';
      const dispatch = createEventDispatcher();

      let data = {
        v1: ...,
        v2: ...
      }
    </script>

    <!-- in the template -->

    <div on:click={ () => dispatch("myEvent", data: data)}>
    ```

    In the parent component

    ```html
    <ChildComponent on:myEvent={ (e) => myEventHandler(e.detail)}>
    ```

13. svelte : reacting to array and object changes

    In the `Todos` component, if we change the completed variable of each todos this way :

    ```js
    todos.forEach((t) => (t.completed = completed));
    ```

    Svelte will not be able to catch the update and change the UI according to the change.

    A rule of thumb is that **the name of the updated variable must appear on the left hand side of an assignment**.

    The previous code should be completed with

    ```js
    todos.forEach((t) => (t.completed = completed));
    todos = todos;
    ```

    The same array updated can also be implement as follows :

    ```js
    todos.forEach((t, i) => (todos[i].completed = completed));
    ```

    todos appears on the left hand side of the assignment of a new value of completed, Svelte will react after this function runs.

    Or the update can be done by providing a new array:

    ```js
    todos = todos.map((t) => {
      return {
        ...t,
        completed: completed,
      };
    });
    ```

    Array.map creates a new array based on another array. Here each element is replaced with a new version having a `completed` parameter replaced by a new value.

    In the code, we chose the 2nd solution as it is shorter.

14. svelte : on:keydown

    Svelte component can event listener to key push. To react to the Escape key :

    ```js
    <Component on:keydown={(e) => e.key == "Escape" && myFunction()}>

    ```

    Component reacts to an Escape key push by calling myFunction after the event key is found to be "Escape".

15. svelte : create a reference pointing to an element

    When an element carries this `bind:this={elementVar}` directive is mounted, it initializes the variable elementVar with a reference to the element having that directive.

16. svelte : component lifecycle

    When a svelte component is initiated, the script part is run first, so the elements in the template do not exist yet so function using them will have an `undefined` reference error.

    Functions needing a template element should be called in the svelte lifecycle element `onMount` that will be executed when the template is mounted to the DOM.

    ```js
    <script>
      import { onMount } from 'svelte';
      ...
      let el;

      onMount( () => {
        ...use of document.getElementById or bind:this element are safe to use inside of onMount
      });
    </script>


    <Component bind:this={el}>

    ```

17. svelte : function needing component update

    If in the course of a function execution, the template is updated and the function needs to _immediately_ use with the updated template, the follow-up use will fail as the component is not updated immediately after any state change. Svelte bundle all the updates to apply them together.

    To carry out these follow-up actions in the function, we need to tell Svelte that other things need to be done after it carried out the updates the function has asked. This reminder is provided by the `tick` function. Sometimes, the follow up actions may not be possible before the change are applied (elements in the template may not yet exist).

    ```js
    <script>
      import { tick } from 'svelte';

      let el;

      function myFunction() {
        v1 = some new value

        await tick();
        // ... the retrieved el is the updated el resulting from the change of prop with the new value of v1
        el.select();
      }

    </script>

    <div bind:this={el} prop={v1}>
    <div>

    ```

18. svelte : passing a boolean prop

    It suffices to pass a prop by its name to a child component expecting this props as a boolean value for the child to receive an initial value for it of `true`.

    ```html
    <!-- In the parent -->
    <Child flag />
    ```

    ```html
    <!-- In the parent -->
    <script>
      export let flag = false;
    </script>
    ```

    Because the parent passed the flag prop, the child will set its prop as true even it the child initializes for itself as false;

    To pass boolean value which is false

    ```js
    <Component flag={false} />
    ```

19. html : Select the text of an input text element

    ```
    ìnput.select()
    ```

    input points to an element in the template.

20. svelte: use:action to add functions component

        Svelte use:action directive add function to run :

        - after the element is added to the DOM
        - aftet the element is removed from the DOM

        ```html
        <Component use:actionFunction></Component>
        ```

        ```js
        actionFunction(node) {
          ... do something with the received node

          return {
            destroy: () => node... ,
          }
        }
        ```

        The action function takes a node as input so we got a reference to the node on which changes are applied.

        The action function should return an object with the property "destroy" which is a function.

        This function will be called by Svelte to run when the Component is removed from the DOM.

        One use case is the removal of event listener after a component is destroyed in order to avoid memory leak :

        ```js
        function selectOnFocus(node) {
          if (node && typeof node.select === 'function' ) {               // make sure node is defined and has a select() method
            const onFocus = event => node.select()                        //
            node.addEventListener('focus', onFocus)                       //

            return {
              destroy: () => node.removeEventListener('focus', onFocus)   // this will be executed when the node is removed from the DOM
            }
          }
        }
        ```

        The focus is a state gained by an input element when navigating by the `Tab` key.

21. svelte : expose child component methods to parent

    a component method is exposed to its parent by :

    - exporting a function definition

    ```js
    <script>
      export function fname() {
        ...
      }
    </script>

    ```

    - exporting a const with a function expression

    ```js
    <script> export const fname = () => {}; </script>
    ```

    Once the function is exposed, with a reference to the child component in the parent component, the child component method can be called simply with a dot notation:

    ```html
    // in the parent component
    <script>
      let child; // set from bind:this directive for example
      child.fname();
    </script>
    ```

## Componentizing a Svelte App

In that chapter of the tutorial, the advice to give a single responsibilty needs clarification. Let's clarify the single responsibility of some of the components.

- The `Todo` component: the single responsility assigned to it is the _control_ of the state of a todo.

  Its responsibilities :

  - Provide means : UI, controls ..., to let the user settle on a new validated state of the todo.
  - Tell the Parent `Todos` component what it should do with the new state by dispatch the proper event.

[mdn-svelte]: https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Svelte_getting_started
