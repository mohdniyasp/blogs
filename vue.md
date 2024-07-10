# Vue Blog

### In vue options are
- data
- computed
- methods
- template
- props

a component is a mini application inside a larger application  
creating component for content (in <script></script>)  
```vue
let app = vue.createApp({});

app.component('page-viewer', {
  template: `
    <div class="container">
      <h1>Page Title</h1>
      <p>Page Content</p>
    </div>
  `
})

app.mount('body')
```
use vue component inside `<body></body>` as an html element  
```vue
<page-viewer></page-viewer>
```
need to pass data into `<page-viewer></page-viewer>`  
componet has props that we pass data to  
can pass data in two different way  
1. as normal html attribute  
    create a prop called page-title and page-content
    ```vue
      <page-viewer
        page-title="Page Title"
        page-content="Page Content"
      ></page-viewer>
    ```
    not enoughf to supply values, have to define inside component (props) (camel-case)
    ```vue
      let app = vue.createApp({});
      
      app.component('page-viewer', {
        props = ['pageTitle', 'pageContent'],
        template: `
          <div class="container">
            <h1>{{ pageTitle }}</h1>
            <p>{{ pageContent }}</p>
          </div>
        `
      })
      
      app.mount('body')
    ```
2. using v-bind directive with props (shorthand - :)  
     - to use string as value use string delemeter (' ')  
        ```vue
          <page-viewer
            :page-title="'Page Title'"
            :page-content="'Page Content'"
          ></page-viewer>
        ```
        not enoughf to supply values, have to define inside component (props) (camel-case)  
        ```vue
          let app = vue.createApp({});
          
          app.component('page-viewer', {
            props = ['pageTitle', 'pageContent'],
            template: `
              <div class="container">
                <h1>{{ pageTitle }}</h1>
                <p>{{ pageContent }}</p>
              </div>
            `
          })
          
          app.mount('body')
        ```
     - to be dynamic  
        ```vue
          <page-viewer
            :page-title="pages[activePage].pageTitle"
            :page-content="pages[activePage].content"
          ></page-viewer>
        ```
        not enoughf to supply values, have to define inside component (props) (camel-case)  
        ```vue
          let app = vue.createApp({});
          
          app.component('page-viewer', {
            props = ['pageTitle', 'pageContent'],
            template: `
              <div class="container">
                <h1>{{ pageTitle }}</h1>
                <p>{{ pageContent }}</p>
              </div>
            `
          })
          
          app.mount('body')
        ```
        - better, one prop, :page  
          ```vue
            <page-viewer
              :page="pages[activePage]"
            ></page-viewer>
          ```
          not enoughf to supply values, have to define inside component (props) (camel-case)  
          ```vue
            let app = vue.createApp({});
            
            app.component('page-viewer', {
              props = ['page'],
              template: `
                <div class="container">
                  <h1>{{ page.pageTitle }}</h1>
                  <p>{{ page.content }}</p>
                </div>
              `
            })
            
            app.mount('body')
          ```
### understanding data flow
creating new app component for navbar  
```vue
app.component('navbar', {
  props: ['pages', 'activePage', 'navLinkClick'],
  template: `
    <nav
      class="navbar navbar-expand-lg bg-body-tertiary"
      :data-bs-theme=[\`\${theme}\`]
    >
      <div class="container-fluid">
        <a class="navbar-brand" href="#">Navbar</a>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li v-for="(page, index) in pages" class="nav-item" :key="index">
              <a 
                class="nav-link"
                :class="{active: activePage == index}"
                aria-current="page"
                :href="page.link.url"
                :title="\`This link goes to the \${page.link.title} page\`"
                @click.prevent="navLinkClick(index)"
              >{{ page.link.text }}</a>
            </li>
          </ul>
          <form class="d-flex">
            <button
              class="btn btn-primary"
              @click.prevent="changeTheme"
            >Toggle Navbar</button>
          </form>
        </div>
      </div>
    </nav>
  `,
  data() {
    return {
      theme: 'light',
    }
  },
  methods: {
    changeTheme() {
      let theme = 'light';
      if (this.theme == theme) {
        theme = 'dark'
      }
      this.theme = theme
    }
  }
});
```
use navbar component as html element  
```vue
  <navbar
    :pages="pages"
    :active-page="activePage"
    :nav-link-click="(index) => activePage = index"
  ></navbar>
```
###### changes
1. escape js template string using '\' (backslash)
2. adding props, props: ['pages', 'activePage']
3. adding navbar attribute which is inside body see next point
4. binding data to props, :pages="pages" :active-page="activePage"
5. move theme method
6. move theme data() {}
7. data flows from parent to child
9. how can child notify parent changes - we use event
10. add :nav-link-click="(index) => activePage = index"
11. add prop ['navLinkClick']
12. props are read only, @click.prevent="navLinkClick(index)"


v-bind -> ':'  
v-on -> '@'  
v-model  
v-if  
v-else  
v-for  
computed() ->  
onMounted()  
watch() async await  
components  
props  
emits  

