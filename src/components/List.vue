<template>
  <div>
    <!-- <ul>
      <li
        v-for="(item, index) in items"
        :key="index"
      >
      <WelcomeItem>
        <template #icon>
          <DocumentationIcon />
        </template>
        <template #heading>{{item.name}}</template>

        Vue’s
        <a href="https://vuejs.org/" target="_blank" rel="noopener">official documentation</a>
        provides you with all information you need to get started.
      </WelcomeItem>

        <h3 class="name">{{item.name}}</h3>
        {{ item }} {{ msg }}
        <span
          class="delete"
          @click="Delete(item.id)"
        >x</span>
      </li>
    </ul> -->
    <Item 
        v-for="(item, index) in Data"
        :key="index">
      <template #icon>
        <DocumentationIcon />
      </template>
      <template #heading>{{ item.name }}</template>

      <span>{{item.id}}</span>
    </Item>
  </div>
  </template>
  
  <script>
  import Item from './Item.vue';
  
  export default {
    components: {
      Item,
    },
    props: {
      Data: Array,
      Delete: Function,
      msg: String,
    },

  }
  </script>

<style scoped>
h3 {
  font-size: 1.2rem;
  font-weight: 500;
  margin-bottom: 0.4rem;
  color: #e7e7e7;
  text-align: left;
}
</style>