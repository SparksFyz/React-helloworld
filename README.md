## React-helloworld

* some demos for learning react
* Just clone then you can play it

## Precompiling JSX

* Install babel
  * `npm install --global babel-cli`
  * Compile Directories
    * `babel src --out-dir build`
  * Piping Files
    * Pipe a file in via stdin and output it to script-compiled.js
    * `babel --out-file script-compiled.js < script.js`


### demo01

```js
<div id="example"></div>
<script type="text/babel">
  ReactDOM.render(
    <h1>hello world</h1>,
    document.getElementById('example')
  );
</script>
```

The template syntax in React is called JSX. It is allowed in JSX to put HTML tags directly into JavaScript codes. ReactDOM.render() is the method which translates JSX into HTML, and renders it into the specified DOM node.

### demo02

> Use JavaScript in JSX

```js
<script type="text/babel">
    var names = ['Alice', 'Bob', 'Sam'];

    ReactDOM.render(
      <div>
      {
        names.map(function (name) {
          return <div>Hello, {name}!</div>
        })
      }
      </div>,
      document.getElementById('example')
    );
</script>
```

You could also use JavaScript in JSX. It takes angle brackets (<) as the beginning of HTML syntax, and curly brackets ({) as the beginning of JavaScript syntax.

### demo03

> Use array in JSX

```js
<script type="text/babel">
    var arr = [
      <h1>Hello world</h1>,
      <h2>react is awesome</h2>,
    ];
    ReactDOM.render(
      <div>{arr}</div>,
      document.getElementById('example')
    );
</script>
```

If a JavaScript variable is array, JSX will implicitly concat all members of the array.

### demo04

> Define a component

```js
<script type="text/babel">
    var HelloMessage = React.createClass({
      render: function() {
        return <h1>Hello {this.props.name}</h1>
      }
    });

    ReactDOM.render(
      <HelloMessage name="John"/>,
      document.getElementById('example')
    );
</script>
```

React.createClass() creates a component class, which implements a render method to return an component instance of the class. You don't need to call new on the class in order to get an instance, just use it as a normal HTML tag.

### demo05

> this.props.children

```js
<script type="text/babel">
    var NotesList = React.createClass({
      render: function() {
        return (
          <ol>
          {
            this.props.children.map(function (child) {
              return <li>{child}</li>
            })
          }
          </ol>
        )
      }
    });

    ReactDOM.render(
      <NotesList>
        <span>hello</span>
        <span>world</span>
      </NotesList>,
      document.body
    );
</script>
```

### demo06

> PropTypes

```js
var MyTitle = React.createClass({
    propTypes: {
      title: React.PropTypes.string.isRequired,
    },
    // P.S. If you want to give the props a default value, use getDefaultProps().
    // getDefaultProps : function () {
    //   return {
    //     title : 'Hello World'
    //   };
    // },
    render: function() {
      return <h1> {this.props.title}</h1>;
    }
});

  var data = '123';

ReactDOM.render(
    <MyTitle title={data} />,
    document.body
);
```

Components have many specific attributes which are called ”props” in React and can be of any type.

Sometimes you need a way to validate these props. You don't want users have the freedom to input anything into your components.

> React has a solution for this and it's called PropTypes.


### demo07

> Finding a DOM node

```js
var MyComponent = React.createClass({
    handleClick: function() {
      this.refs.myTextInput.focus();
    },
    render: function() {
      return (
        <div>
          <input type="text" ref="myTextInput" />
    <input type="button" value="Focus the text input" onClick={this.handleClick} />
        </div>
      );
    }
});

ReactDOM.render(
  <MyComponent />,
  document.getElementById('example')
);
```
Sometimes you need to reference a DOM node in a component.

React gives you the ref attribute to find it.

> The desired DOM node should have a ref attribute, and this.refs.[refName] would return the corresponding DOM node. Please be minded that you could do that only after this component has been mounted into the DOM, otherwise you get null.

### demo08

> this.state

```js
var LikeButton = React.createClass({
    getInitialState: function() {
      return {liked: false};
    },
    handleClick: function(event) {
      this.setState({liked: !this.state.liked});
    },
    render: function() {
      var text = this.state.liked ? 'like' : 'haven\'t liked';
      return (
        <p onClick={this.handleClick}>
          You {text} this. Click to toggle.
        </p>
      );
    }
});

ReactDOM.render(
    <LikeButton />,
    document.getElementById('example')
);
```

### demo09

> Form

```js
var Input = React.createClass({
    getInitialState: function() {
      return {value: 'Hello'};
    },
    handleChange: function(event) {
      this.setState({value: event.target.value});
    },
    render: function() {
      var value = this.state.value;
      return (
        <div>
          <input type="text" value={value} onChange={this.handleChange} />
          <p>{value}</p>
        </div>
      );
    }
});
ReactDOM.render(<Input/>, document.body);
```

According to React's design philosophy, this.state describes the state of component and is mutated via user interactions, and this.props describes the properties of component and is stable and immutable.

Since that, the value attribute of Form components, such as `<input>`, `<textarea>`, and `<option>`, is unaffected by any user input. If you wanted to access or update the value in response to user input, you could use the onChange event.

### demo10

> Component Lifecycle

```js
var Hello = React.createClass({
    getInitialState: function() {
      return {
        opacity: 1.0
      };
    },

    componentDidMount: function() {
      this.timer = setInterval(function () {
        var opacity = this.state.opacity;
        opacity -= .05;
        if (opacity < 0.1) {
          opacity = 1.0;
        }
        this.setState({
          opacity: opacity
        });
      }.bind(this),100);
    },
    render: function () {
      return (
        <div style={{opacity: this.state.opacity}}>
          Hello {this.props.name}
        </div>
      );
    }
});

ReactDOM.render(
    <Hello name="world"/>,
    document.body
);
```

Components have three main parts of their lifecycle: Mounting(being inserted into the DOM), Updating(being re-rendered) and Unmounting(being removed from the DOM). React provides hooks into these lifecycle part. will methods are called right before something happens, and did methods which are called right after something happens.


> The following is a whole list of lifecycle methods.

+ componentWillMount()
+ componentDidMount()
+ componentWillUpdate(object nextProps, object nextState)
+ componentDidUpdate(object prevProps, object prevState)
+ componentWillUnmount()
+ componentWillReceiveProps(object nextProps): invoked when a mounted component receives new props.
+ shouldComponentUpdate(object nextProps, object nextState): invoked when a component decides whether any changes warrant an update to the DOM.


