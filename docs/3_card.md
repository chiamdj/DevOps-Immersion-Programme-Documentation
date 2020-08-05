# 3. Our First Component

The first component we will develop is the Card.

Cards are surfaces that display content and actions on a single topic.

They should be easy to scan for relevant and actionable information. Elements, like text and images, should be placed on them in a way that clearly indicates hierarchy.

In this project, we use the `Card` component as a surface for all our other components. For more information on the `Card` component, see [https://material-ui.com/components/cards/](https://material-ui.com/components/cards/).


## 3.1 HTML Configuration
Before we start creating any React components, adding the following code in the `index.html` file between the `header` and `body` tags:
```HTML
<style>
    html,
    body,
    #root,
    #root > div {
      height: 100%;
      margin: 0;
    }
  </style>
```

This code block removes all the margins in the React App (defaults to 8px) and stretches the App to fill 100% of the browser height.

Next, like HTML, we also create a separate `.css` file to store our CSS styles for our React components. Create a file `App.css` in the same directory as `App.js` and import it before the function `App`:

```JS
import "./App.css"
```

## 3.2 `App.js`

Navigate to the `App.js` file in the Files directory. Note that you should see "Edit src/App.js and save to reload." in the Browser. Also note the boilerplate code already included:

```JS
import React from "react";
import "./styles.css";

export default function App() {
  return (
    <div className="App">
      <h1>Hello CodeSandbox</h1>
      <h2>Start editing to see some magic happen!</h2>
    </div>
  );
}
```

There are two parts to the code:

+ **Importing components**: React is imported into the JavaScript file. This is necessary to distinguish it from non-React JS files. Other CSS and image files are also imported.
+ **App Function**: Contains JSX code, which allows us to write HTML in React in order to produce React elements. It is neither HTML nor JS.  You can read more about JSX [here](https://reactjs.org/docs/introducing-jsx.html). We also set this function as the default export for `App.js`.

!!! info "Where is the function `App` consumed?"
	This App function is imported in `index.js`, where it is rendered to the React DOM here at the 'root' HTML DOM node. 

	```JS
	ReactDOM.render(
	<React.StrictMode>
		<App />
	</React.StrictMode>,
	document.getElementById('root')
	);
	```

	Read more about rendering React elements to the DOM [here](https://reactjs.org/docs/rendering-elements.html).

## 3.3 Rendering a Card

First, delete everything within the `div` tag.

```JSX
import React from 'react';
import logo from './logo.svg';
import './App.css';

function App() {
    return (
      <div className="App">
      </div>
    );
  }
export default App;
```

!!! question "What is `className`?"
	`className` is a prop used in React for adding CSS classes to React components. You can define 	your styles in a separate `*.css` file as usual and refer to them using `className`.

!!! question "What are React props?"
	Props are arguments passed into React components. React Props are like function arguments in JavaScript and attributes in HTML.
	
	To send props into a component, use the [same syntax](https://www.w3schools.com/react/react_props.asp) as HTML attributes:

Next, add the following CSS styles to `App.css`:

```CSS
.App {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #80cbc4;
}
```

Next, refer to the [Material-UI documentation](https://material-ui.com/components/cards/) on the `Card` component. Study the sample code for a **Simple Card**.

```JSX
import React from 'react';
import { makeStyles } from '@material-ui/core/styles';
import Card from '@material-ui/core/Card';
import CardActions from '@material-ui/core/CardActions';
import CardContent from '@material-ui/core/CardContent';
import Button from '@material-ui/core/Button';
import Typography from '@material-ui/core/Typography';

const useStyles = makeStyles({
  root: {
    minWidth: 275,
  },
  bullet: {
    display: 'inline-block',
    margin: '0 2px',
    transform: 'scale(0.8)',
  },
  title: {
    fontSize: 14,
  },
  pos: {
    marginBottom: 12,
  },
});

export default function SimpleCard() {
  const classes = useStyles();
  const bull = <span className={classes.bullet}>•</span>;

  return (
    <Card className={classes.root}>
      <CardContent>
        <Typography className={classes.title} color="textSecondary" gutterBottom>
          Word of the Day
        </Typography>
        <Typography variant="h5" component="h2">
          be{bull}nev{bull}o{bull}lent
        </Typography>
        <Typography className={classes.pos} color="textSecondary">
          adjective
        </Typography>
        <Typography variant="body2" component="p">
          well meaning and kindly.
          <br />
          {'"a benevolent smile"'}
        </Typography>
      </CardContent>
      <CardActions>
        <Button size="small">Learn More</Button>
      </CardActions>
    </Card>
  );
}
```
Now we only need the Card surface and not the content. We can simplify the code as follows:
```JSX
<Card>
	<CardContent>
	</CardContent>
	<CardActions>
	</CardActions>
</Card>
```

Now this to `App.js` within the `div` tag.
```JSX
import React from "react";
import "./App.css";
import { Card } from "@material-ui/core";

function App() {
  return (
    <div className="App">
      <Card className="card">
        <CardContent />
        <CardActions />
      </Card>
    </div>
  );
}

export default App;

```
You will notice errors popping up immediately, stating that React components like `Card` are missing. To fix this, add the necesary imports to the top, below the first line (where React is imported):

```JS
import { Card, CardContent, CardActions } from "@material-ui/core";
```

Finally, add the following styles to `App.css`. This will center the component vertically and horizontally, and change the background color.

```CSS
.card {
  width: 500px;
  height: 500px;
  padding: 10px
}
```
To apply these CSS styles on the `Card` component, add a `className` prop it referencing `.card`:
```JSX
<Card className="card">
```

## 3.4 Final Result
Your final code and App should look like this:
```JSX
import React from "react";
import "./App.css";
import { Card, CardContent, CardActions } from "@material-ui/core";

function App() {
  return (
    <div className="App">
      <Card className="card">
        <CardContent />
        <CardActions />
      </Card>
    </div>
  );
}

export default App;
```
![3_progress](img/3_progress.JPG)
