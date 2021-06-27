<template>
    <div class="navigation">
      <nav>
        <ul role="menu">
          <li v-for="(navItem, index) in pages" :key="index">
            <nuxt-link :to="'/' + navItem.fields.slug.trim()" role="menuitem">{{navItem.fields.navTitle}}</nuxt-link>
          </li>
        </ul>
      </nav>
    </div>
</template>

<script>
    export default {
        name: 'Navigation',
        props: {
          navItems: {
            type: Array, // We expect an array of pages that we need for our navigation
            required: true
          }
        },
        data: function (){
            return {
                pages: this.navItems
            }
            
        }
    }
</script>

<style>
  .navigation {
    background-color:red;
    padding: 50px 0;
  }

   ul {
      padding:0;
      margin:0;
      list-style:none;
    }
    li {
      display:inline-block;
      padding: 0 10px;
    }
    a {
      color:#333;
      text-decoration: none;
    }
</style>
