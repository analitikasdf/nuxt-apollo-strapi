<template>
  <section class="py-24">
    <div class="container mx-auto">
      <div class="flex justify-center">
        <div class="w-full md:w-1/2 flex flex-col items-center mx-auto bg-gray-200 p-8">
          <h3 class="text-3xl mb-8">Login Page</h3>
          <div class="w-full max-w-xs">
            <form @submit.prevent="login()" class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
              <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="username">Username</label>
                <input
                  v-model="form.identifier"
                  class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                  id="username"
                  type="text"
                  placeholder="Username"
                />
              </div>
              <div class="mb-6">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="password">Password</label>
                <input
                  v-model="form.password"
                  class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline"
                  id="password"
                  type="password"
                  placeholder="******************"
                />
              </div>
              <div class="flex items-center justify-between">
                <button
                  class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
                  type="submit"
                >Sign In</button>
                <a
                  class="inline-block align-baseline font-bold text-sm text-blue-500 hover:text-blue-800"
                  href="#"
                >Forgot Password?</a>
              </div>
            </form>
            <p class="text-center text-gray-500 text-xs">&copy;2020 Acme Corp. All rights reserved.</p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>


<script>
import gql from "graphql-tag";
export default {
  data() {
    return {
      form: {
        identifier: "",
        password: ""
      }
    };
  },
  methods: {
    async login() {
      const credentials = this.form;
      try {
        const { data: { login: { user, jwt } } } = await this.$apollo.mutate({
          mutation: gql`
            mutation($identifier: String!, $password: String!) {
              login(input: { identifier: $identifier, password: $password }) {
                user {
                  id
                  username
                  email
                  role {
                    name
                    type
                    description
                  }
                }
                jwt
              }
            }
          `,
          variables: credentials
        })
        //set the jwt to the this.$apolloHelpers.onLogin
        await this.$apolloHelpers.onLogin(jwt)
      } catch (e) {
        console.error(e)
      }
    }
  },
  async mounted(){
    //clear apollo-token from cookies to make sure user is fully logged out
    await this.$apolloHelpers.onLogout() 
  }
}
</script>