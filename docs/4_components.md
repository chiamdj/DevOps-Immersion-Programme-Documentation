# 4. Adding more Components

Now we are ready to add more components to our `Card`.

We will be adding the following components:

+ [Typography](https://material-ui.com/components/typography/)
+ [Divider](https://material-ui.com/components/dividers/)
+ [List](https://material-ui.com/components/lists/)
+ [Button](https://material-ui.com/components/buttons/)

Click on the respective components to read more about them.

## 4.1 Adding a Title

Import the `Typography` component from `@material-ui/core` and add the `Typography` component between the `<CardContent> tags`:

```JSX
<CardContent>
    <Typography variant="h4">Shopping List</Typography>
</CardContent>
```  

!!! note
	`<CardContent />` is the same as `<CardContent><CardContent/>`. The former shortens code and improves readability when a React component has no props.

Next import and add a `Divider` component below it:

```JSX
<Divider />
```  

You should see a title with a horizontal rule on the `Card`.

## 4.2 Adding a List

Before we create a list, we should first define an array of items that will be displayed in the list. 

Add the following line of code before the function `App`:

```JS
let listItems = ['apple', 'banana', 'orange']
```

Before adding the `List` component it is good to reference the Material-UI documentation to see the structure of the component. Go to [https://material-ui.com/components/lists/](https://material-ui.com/components/lists/) and check out the code for the **Simple List**.

Note the hierarchy of components within the the `List` component:
```JSX
<List component="nav" aria-label="main mailbox folders">
	<ListItem button>
		<ListItemIcon>
			<InboxIcon />
		</ListItemIcon>
		<ListItemText primary="Inbox" />
	</ListItem>
	<ListItem button>
		<ListItemIcon>
			<DraftsIcon />
		</ListItemIcon>
		<ListItemText primary="Drafts" />
	</ListItem>
</List>
```

Next, import the `List` component and add it below the `Divider` component. Within that component, we want to iterate over each item in `listItems` to display it in the list as a text. We do this by using the JavaScript `map` function.

!!! help
	See [https://medium.com/poka-techblog/simplify-your-javascript-use-map-reduce-and-filter-bd02c593cc2d](https://medium.com/poka-techblog/simplify-your-javascript-use-map-reduce-and-filter-bd02c593cc2d) for a good explanation on the `map` function

```JS
<List>
	{listItems.map(item => (
		<ListItem>
			<ListItemText>{item}</ListItemText>
		</ListItem>
	))}
</List>
```

Let's also add an icon for each element in the list. Import `<ShoppingCartIcon />` from `@material-ui/icons` and add it below the `ListItemText` component.

```JSX
import ShoppingCartIcon from "@material-ui/icons/ShoppingCart";
// Code omitted for brevity
<List>
	{listItems.map(item => (
		<ListItem>
			<ListItemIcon>
				<ShoppingCartIcon />
			</ListItemIcon>
			<ListItemText>{item}</ListItemText>
		</ListItem>
	))}
</List>
```

We also want to allow users to remove the items in the list.

Import the `DeleteForeverIcon` icon and add it below the `ListItemText`:
```JSX
import DeleteForeverIcon from "@material-ui/icons/DeleteForever";
// Code omitted for brevity
<List>
	{listItems.map(item => (
		<ListItem>
			<ListItemIcon>
				<ShoppingCartIcon />
			</ListItemIcon>
			<ListItemText>{item}</ListItemText>
			<DeleteForeverIcon style={{ color: "red" }} />
		</ListItem>
	))}
</List>
```


## 4.3 Adding a Button

Finally, expand the `CardActions` component and import the `Button` component. Add the following line of code to render the button:

```JSX
<CardActions>
	<Button size="small">
		Add Item
	</Button>
</CardActions>
```

Finally, go to `App.css` and remove the `height` property from `.Card`.

You are done for this section. Note however that the list is not interactive, i.e. clicking on the buttons and the list does not do anything. React will allow us to "interactive" 