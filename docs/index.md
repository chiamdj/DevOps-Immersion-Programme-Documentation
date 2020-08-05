# 1. Setting up the environment 

The first step to any React project is to install React and install any project dependencies we might need. 

## 1.1 CodeSandbox

We will be creating this project on CodeSandbox. Head on to [https://codesandbox.io](https://codesandbox.io) and create a new React sandbox.

!!! error "Save your work"
    In order to avoid losing your work, it is recommended that you create a [CodeSandbox account](https://codesandbox.io/signin) before moving on. You need a [GitHub account](https://github.com/join) to sign up.

!!! note "Creating a local Repository"
    The repository you are forking comes from `create-react-app`, which is an officially supported way to create single-page React applications. See [Create-React-App documentation](https://create-react-app.dev/docs/getting-started). 
    
    If you wish to use a Desktop Code IDE, you can install `create-react-app` to your project directory with the following code (replace `my-app` with the name of your app):

    === "npm"
        ```bash
        npm init react-app my-app   
        ```
    === "yarn"
        ```bash
        yarn create react-app my-app
        ```
Next, examine the project directory under the Files tab on the left. You should see something like the following:

```
  package.json
  public/
    index.html
  src/
    App.js
    index.js
    styles.css
```
If you create a React project locally on your computer, there will be some additional files.

For the project to build, these files must exist with **exact** filenames:

+ `public/index.html` is the page template;
+ `src/index.js` is the JavaScript entry point.
You can delete or rename the other files.

!!! info "Some Additional Information"
    You may create subdirectories inside src. For faster rebuilds, only files inside src are processed by webpack. You need to put any JS and CSS files inside src, otherwise webpack won’t see them.

    Only files inside public can be used from public/index.html. Read instructions below for using assets from JavaScript and HTML.

    You can, however, create more top-level directories. They will not be included in the production build so you can use them for things like documentation.

Look through each of the files and try to infer what each file does.

## 1.2 Material-UI

![Google's Material Design](https://images.fastcompany.net/image/upload/w_1280,f_jpg,q_auto,fl_lossy/wp-cms/uploads/2018/05/1-google-announces-the-sequel-to-material-design-material-theming-1.gif)
Material-UI contains react components for faster and easier web development. It is a React UI framework inspired by Google's [Material Design](https://material.io/design) as illustrated above, and provides a nice user interface for our react app.

Material-UI is available as an `npm` package. See [instructions to install](https://material-ui.com/getting-started/installation/) to install Material-UI on your local project directory. On CodeSandbox, you can add Material-UI as a dependency by expanding the dependency blade, click add dependency and search/install the following packages:
+ 'material-ui/core'
+ 'material-ui/icons'

When completed, move on to the next step where we set up the foundation of the app.