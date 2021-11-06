<template>
  <div class="container mx-auto">
    <div class="flex">
      <div class="w-full p-8 bg-lime-300 text-fuchsia-500">
        <pre class="p-8 bg-gray-100">{{ JSON.stringify(orders, null, 2) }}</pre>
      </div>
    </div>
  </div>
</template>

<script>
import gql from 'graphql-tag'
export default {
  middleware: ['authenticated'],
  data(){
    return {
      orders: {}
    }
  },
  apollo: {
    orders: {
      query: gql`
        query {
          orders{
            name
          }
        }
      ` 
    }
  }
}
</script>