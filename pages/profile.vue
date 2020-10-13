<template>
  <div>
    <amplify-authenticator v-if="authState !== 'signedin'" class="container" />
    <div v-if="authState === 'signedin' && user">
      <h1>Hello, {{user.username}} V2</h1>
      <button v-on:click="signOut">Sign Out</button>
    </div>
  </div>
</template>

<script>
import { onAuthUIStateChange } from '@aws-amplify/ui-components'
import { Auth }  from 'aws-amplify';

export default {
  created() {
    onAuthUIStateChange((authState, authData) => {
      this.authState = authState;
      this.user = authData;
    })
  },
  data() {
    return { user: undefined, authState: undefined }
  },
  methods: {
    signOut: async () => Auth.signOut()
  },
  beforeDestroy() {
    return onAuthUIStateChange;
  }
}
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
}

:root {
  --amplify-primary-color: #4287f5;
  --amplify-primary-tint: #005cf0;
  --amplify-primary-shade: #005cf0;
}
</style>
