
## Developing

This article outlines how to get Dashy running in a development environment, and outlines the basics of the architecture. 

- [Setting up the Development Environment]()
- [Resources for Beginners]()
- [Style Guide]()
- [Frontend Components]()
- [Project Structure]()

### Setting up the Dev Environment

#### Prerequisites
You will need either the latest or LTS version of **[Node.js](https://nodejs.org/)** to build and serve the application and **[Git](https://git-scm.com/downloads)** to easily fetch the code, and push any changes. If you plan on running or deploying the container, you'll also need **[Docker](https://docs.docker.com/get-docker/)**. To avoid any unexpected issues, ensure you've got at least **[NPM](https://www.npmjs.com/get-npm)** V 7.5 or **[Yarn](https://classic.yarnpkg.com/en/docs/install/#windows-stable)** 1.22 (you may find [NVM](https://github.com/nvm-sh/nvm) helpful for switching/ managing versions).

#### Running the Project

1. Get Code: `git clone git@github.com:Lissy93/dashy.git`
2. Navigate into the directory: `cd dashy`
3. Install dependencies: `yarn`
4. Start dev server: `yarn dev`

Dashy should now be being served on http://localhost:8080/. Hot reload is enabled, so making changes to any of the files will trigger them to be rebuilt and the page refreshed.

#### Project Commands

- `yarn dev` - Starts the development server with hot reloading
- `yarn build` - Builds the project for production, and outputs it into `./dist`
- `yarn start` - Starts a web server, and serves up the production site from `./dist`
- `yarn validate-config` - Parses and validates your `conf.yml` against Dashy's schema
- `yarn lint` - Lints code to ensure it follows a consistent, neat style
- `yarn test` - Runs tests, and outputs results

There is also:
- `yarn build-and-start` will run `yarn build` and `yarn start`
- `yarn build-watch` will output contents to `./dist` and recompile when anything in `./src` is modified, you can then use either `yarn start` or your own server, to have a production environment that watches for changes.

Note:
- If you are using NPM, replace `yarn` with `npm run`
- If you are using Docker, precede each command with `docker exec -it [container-id]`. Container ID can be found by running `docker ps`

### Resources for Beginners
New to Web Development? Glad you're here! Dashy is a pretty simple app, so it should make a good candidate for your first PR. Presuming that you already have a basic knowledge of JavaScript, the following articles should point you in the right direction for getting up to speed with the technologies used in this project:
- [Introduction to Vue.js](https://v3.vuejs.org/guide/introduction.html)
- [Vue.js Walkthrough](https://www.taniarascia.com/getting-started-with-vue/)
- [Definitive guide to SCSS](https://blog.logrocket.com/the-definitive-guide-to-scss/)
- [Complete beginners guide to Docker](https://docker-curriculum.com/)
- [Docker Classroom - Interactive Tutorials](https://training.play-with-docker.com/)
- [Quick start TypeScript guide](https://www.freecodecamp.org/news/learn-typescript-in-5-minutes-13eda868daeb/)
- [Complete TypeScript tutorial series](https://www.typescripttutorial.net/)
- [Using TypeScript with Vue.js](https://blog.logrocket.com/vue-typescript-tutorial-examples/)
- [Git cheat sheet](http://git-cheatsheet.com/)
- [Basics of using NPM](https://www.freecodecamp.org/news/what-is-npm-a-node-package-manager-tutorial-for-beginners/)

As well as Node, Git and Docker- you'll also need an IDE (e.g. [VS Code](https://code.visualstudio.com/) or [Vim](https://www.vim.org/)) and a terminal (Windows users may find [WSL](https://docs.microsoft.com/en-us/windows/wsl/) more convenient). 

### Style Guide

Linting is done using [ESLint](https://eslint.org/), and using the [Vue.js Styleguide](https://github.com/vuejs/eslint-config-standard), which is very similar to the [AirBnB Stylguide](https://github.com/airbnb/javascript). You can run `yarn lint` to report and fix issues. While the dev server is running, issues will be reported to the console automatically. Any lint errors will trigger the build to fail. Note that all lint checks must pass before any PR can be merged.

The most significant things to note are:
- Indentation should be done with two spaces
- Strings should use single quotes
- All statements must end in a semi-colon
- The final element in all objects must be preceded with a comma
- Maximum line length is 100
- There must be exactly one blank line between sections, before function names, and at the end of the file
- With conditionals, put else on the same line as your if block’s closing brace
- All multiline blocks must use braces
- Avoid console statements in the frontend

For the full styleguide, see: [github.com/airbnb/javascript](https://github.com/airbnb/javascript) 


### Frontend Components

All frontend code is located in the `./src` directory, which is split into 5 sub-folders:
- Components - All frontend web components are located here. Each component should have a distinct, well defined and simple task, and ideally should not be too long. The components directory is organised into a series of sub-directories, representing a specific area of the application
  - PageStrcture - Components relating to overall page structure (nav, footer, etc)
  - FormElements - Reusable form elements (button, input field, etc)
  - LinkItems - Components relating to Dashy's sections and items (item group, item, item icon, etc)
  - Configuration - Components relating to Dashy's configuration forms (cloud backup, JSON editor, etc)
- Views - Each view directly corresponds to a route (defined in the router), and in effectively a page. They should have minimal logic, and just contain a few components
- Utils - These are helper functions, or logic that is used within the app does not include an UI elements
- Styles - Any SCSS that is used globally throughout that app, and is not specific to a single component goes here. This includes variables, color themes, typography settings, CSS reset and media queries
- Assets - Static assets that need to be bundled into the application, but do not require any manipulation go here. This includes interface icons and fonts

The structure of the components directory is similar to that of the frontend application layout

<p align="center"><img src="https://i.ibb.co/wJCt0Lq/dashy-page-structure.png" width="500"/></p>

### Common Tasks

#### Creating a new theme

See [Theming](./theming.md)

#### Adding a new option in the config file

All application config is specified in `./public/conf.yml` - see [Configuring Docs](./configuring.md) for info. Before adding a new option in the config file, first ensure that there is nothing similar available, that is is definitely necessary, it will not conflict with any other options and most importantly that it will not cause any breaking changes. Ensure that you choose an appropriate and relevant section to place it under.

Checklist:
- Update the [Schema](https://github.com/Lissy93/dashy/blob/master/src/utils/ConfigSchema.js) with the parameters for your new option
- Set a default value (if required) within [`defaults.js`](https://github.com/Lissy93/dashy/blob/master/src/utils/defaults.js)
- Document the new value in [`configuring.md`](./configuring.md)
- Test that the reading of the new attribute is properly handled, and will not cause any errors when it is missing or populated with an unexpected value

### Directory Structure

#### Files in the Root: `./`
```
╮
├── package.json        # Project meta-data, dependencies and paths to scripts
├── src/                # Project front-end source code
├── server.js           # A Node.js server to serve up the /dist directory
├── vue.config.js       # Vue.js configuration
├── Dockerfile          # The blueprint for building the Docker container
├── docker-compose.yml  # A Docker run command
├── .env                # Location for any environmental variables
├── yarn.lock           # Auto-generated list of current packages and version numbers
├── docs/               # Markdown documentation
├── README.md           # Readme, basic info for getting started
├── LICENSE.md          # License for use
╯
```

#### Frontend Source: `./src/`

```
./src
├── App.vue                       # Vue.js starting file
├── assets                        # Static non-compiled assets
│  ├── fonts                      # .ttf font files
│  ╰── interface-icons            # SVG icons used in the app 
├── components                    # All front-end Vue web components
│  ├── Configuration              # Components relating to the user config pop-up
│  │  ├── CloudBackupRestore.vue  # Form where the user manages cloud sync options
│  │  ├── ConfigContainer.vue     # Main container, wrapping all other config components
│  │  ├── CustomCss.vue           # Form where the user can input custom CSS
│  │  ├── EditSiteMeta.vue        # Form where the user can edit site meta data
│  │  ╰── JsonEditor.vue          # JSON editor, where the user can modify the main config file
│  ├── FormElements               # Basic form elements used throughout the app
│  │  ├── Button.vue              # Standard button component
│  │  └── Input.vue               # Standard text field input component
│  ├── LinkItems                  # Components for Sections and Link Items
│  │  ├── Collapsable.vue         # The collapsible functionality of sections
│  │  ├── IframeModal.vue         # Pop-up iframe modal, for viewing websites within the app
│  │  ├── Item.vue                # Main link item, which is displayed within an item group
│  │  ├── ItemGroup.vue           # Item group is a section containing icons
│  │  ├── ItemIcon.vue            # The icon used by both items and sections
│  │  ╰── ItemOpenMethodIcon.vue  # A small icon, visible on hover, indicating opening method 
│  ├── PageStrcture               # Components relating the main structure of the page
│  │  ├── Footer.vue              # Footer, visible at the bottom of all pages
│  │  ├── Header.vue              # Header, visible at the top of pages, and includes title and nav
│  │  ├── Nav.vue                 # Navigation bar, includes a list of links
│  │  ╰── PageTitle.vue           # Page title and sub-title, visible within the Header
│  ╰── Settings                   # Components relating to the quick-settings, in the top-right
│     ├── ConfigLauncher.vue      # Icon that when clicked will launch the Configuration component
│     ├── ItemSizeSelector.vue    # Set of buttons used to set and save item size
│     ├── KeyboardShortcutInfo.vue# Small pop-up displaying the available keyboard shortcuts
│     ├── LayoutSelector.vue      # Set of buttons, letting the user select their desired layout
│     ├── SearchBar.vue           # The input field in the header, used for searching the app
│     ├── SettingsContainer.vue   # Container that wraps all the quick-settings components
│     ╰── ThemeSelector.vue       # Drop-down menu enabling the user to select and change themes
├── main.js                       # Main front-end entry point
├── registerServiceWorker.js      # Registers and manages service workers, for PWA apps
├── router.js                     # Defines all available application routes
├── styles                        # Directory of all globally used common SCSS styles
├── utils                         # Directory of re-used helper functions
│  ├── ArrowKeyNavigation.js      # Functionality for arrow-key navigation
│  ├── CloudBackup.js             # Functionality for encrypting, processing and network calls
│  ├── ConfigSchema.json          # The schema, used to validate the users conf.yml file
│  ├── ConfigValidator.js         # A helper script that validates the config file against schema
│  ├── defaults.js                # Global constants and their default values
│  ├── ErrorHandler.js            # Helper function called when an error is returned
│  ├── JsonToYaml.js              # Function that parses and converts raw JSON into valid YAML
│  ╰── ThemeHelper.js             # Function that handles the fetching and setting of user themes
╰── views                         # Directory of available pages, corresponding to available routes
   ╰── Home.vue                   # The home page container
```