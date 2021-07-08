# A TODO List In OOHTML
Below is a TODO list application that is based a plain JavaScript array bound to `document.state`.

It features the ability to add items and toggle the active state of each item.
+ **The *add* feature** - When the *add* button in the TODO container is clicked, an item is added to the array.
+ **The *toggle* feature** - When the *toggle* button of an item is clicked, the corresponding entry in the todo list gets its `active` property set to `true/false`.

> Note that we're involving the *Observer API* to reactively mutate the todo list. Also, we're using the *Play UI* to manipulate the DOM.

```html
<html>

    <head>

        <title>A TODO List in OOHTML</title>
        <meta name="oohtml" content="attr.id=data-id" />

        <script src="https://unpkg.com/@webqit/play-ui/dist/main.js"></script>
        <script src="https://unpkg.com/@webqit/oohtml/dist/main.js"></script>
        <script>
            // Make PlayUI available globally
            window.$ = window.WebQit.$;
            window.Observer = window.WebQit.Observer;

            // Create the app
            const app = {
                title: 'My TODO',
                todo: [
                    {desc: 'Task-1', active: true},
                    {desc: 'Task-2', active: true},
                    {desc: 'Task-3', active: true},
                ],
            };
            document.setState(app);
        </script>

        <template name="items">
            
            <li namespace>
                <span data-id="desc"></span>
                <button data-id="toggle">Toggle Active</button>
                <script type="subscript">
                    $(this.namespace.desc).html(this.state.desc);
                    $(this.namespace.desc).css('opacity', this.state.active ? '1' : '0');
                    $(this.namespace.toggle).on('click', () => {
                        this.state.active = !this.state.active;
                    });
                </script>
            </li>

        </template>

    </head>

    <body>

        <div namespace>

            <h2 data-id="title"></h2>
            <ol data-id="items" template="items"></ol>
            <button data-id="add">Add</button>
            
            <br /><br />

            <div>
            You can also add items from the console directly.<br />
            Open your console and type: <code>Observer.proxy(document.state.todo).push({desc:"New Item", active: true})</code>
            </div>

            <script type="subscript">
                this.namespace.title.innerHTML = document.state.title;
                $(this.namespace.items).itemize(document.state.todo);
                this.namespace.add.addEventListener('click', () => {
                    Observer.proxy(document.state.todo).push({desc: prompt('Task description', , 'Task-' + (document.state.todo.length + 1)), active: true,});
                });
            </script>

        </div>
    </body>

</html>
```
 
<a href="/html/tooling/examples/todo.html" target="_blank">Check the live demo here</a> or copy and paste the code in a blank HTML page and view in your browser.