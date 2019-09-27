---
title: Update DOM Elements Outside Vue
date: "2019-08-02"
template: "post"
draft: false
slug: "/posts/update-dom-elements-outside-vue/"
category: "Vue"
tags:
  - "Vue"
  - "Javascript"
description: "Update content that lives outside Vue from within Vue. Working on a legacy PHP application that's, getting converted to an SPA using Vue, I encountered a need to interact with some sections of the application that reside outside of Vue."
---

### Challenge

Update content that lives outside Vue from within Vue.

### Scenario

Working on a legacy PHP application that's, getting converted to an SPA using Vue, I encountered a need to interact with some sections of the application that reside outside of Vue. In particular the page heading and icons are set by the PHP controllers. With a couple sections converted over to Vue, we're able to route requests between the new Vueified sections using Vue Router. This works great but leaves the page heading set to the previous section.

Eventually this section will likely be converted to Vue as well but for now that's out of scope. A quick fix would be to just not use router links, but I don't want to give up the benefits of routing between these sections using Vue Router.

### Existing Options

Doing some DOM manipulation using some vanilla Javascript (getElementById) was the first option that came to mind but I wanted to see what some others have done in similar situations.

#### Portal-Vue

I found a package called Portal-Vue and gave it a shot. I came across this package from a post on [alligator.io](<https://alligator.io/vuejs/portal-vue/>).

I couldn't find my particular use-case (manipulate DOM outside Vue) in the Portal-Vue documentation but the article on alligator.io covered this. 

1. Install portal-vue 
   ```npm install portal-vue --save```

2. Enable plugin

   ```
   import PortalVue from 'portal-vue';
   Vue.use(PortalVue);
   ```

3. Create new Vue component

   ```
   <template>
     <div>
       <portal target-el="#target-element-id">
         <p>Contents for target element here</p>
       </portal>
     </div>
   </template>
   ```

4. Add component to the page

   ```
   <NewPortalComponent />
   ```

   This works fine, but after doing all this and thinking about what's actually happening, I can just interact with the DOM myself and avoid installing another package and adding it as a global Vue plugin.

### Solution

To keep this lightweight I decided to create a component specifically for my use-case.

#### Renderless Vue Component

Renderless as this doesn't need to render anything where the component is added. Unlike portal-vue, I'll just set the HTML for the target element directly in the script so the component won't need a template.

##### Requirements

- Find an element on the page and update the inner HTML
- HTML will be different on each page so content will need to be dynamic

##### Steps

1. Wrap target element in div with id

   ```html
   <div id="page-heading">
     <h1><i class="original-icon"> Original Page Heading</h1>
   </div>
   ```

2. Create renderless component and get target element

   ```vue
   <script>
     mounted() {
         let target = document.getElementById('page-heading');
     }
     render() {
         return null;
     }
   </script>
   ```

3. Set new content for element in mounted:

   ```javascript
   if (target) {
     target.innerHTML = '<h1><i class="new-icon"> New Header</h1>';
   }
   ```

4. Add new content as props to make this dynamic

   ```javascript
   props: {
     icon: {
       type: String,
       required: true
     },
     heading: {
       type: String,
       required: true
     }
   },
   ```

5. Update new content to use props

   ```javascript
   mounted() {
     let target = document.getElementById('page-heading');
     if (target) {
       target.innerHTML = `<h1><i class="{this.icon}"> {this.heading}</h1>`;
     }
   }
   ```

##### Finished component

```vue
/** ../components/UpdateHeading.vue */
<script>
  props: {
    icon: {
      type: String,
      required: true
    },
    heading: {
      type: String,
      required: true
    }
  },
  mounted() {
      let target = document.getElementById('page-heading');
      if (target) {
        target.innerHTML = `<h1><i class="{this.icon}"> {this.heading}</h1>`;
      }
  }
  render() {
      return null;
  }
</script>
```

#### Usage

To use the component: add the component to the page and pass in the desired icon and heading. When the component is mounted it will update the contents of the target element with your props.

```vue
<UpdateHeading
  icon="new-icon"
  heading="New Page Heading"
/>
```

#### Next Steps

The final component got refactored a bit more by moving the updating of innerHTML into a method that is called in mounted and then adding some watchers on the props which also call the new method (not needed for the use-case in the app but setting it up this way allowed the component to be interactive in Storybook using knobs).

This was a specific use-case for the application I was working on, but hopefully this gives some insight into a couple different approaches to tackling DOM manipulation outside of Vue from within Vue.

