<template>
  <footer>
    <router-link to="/">Home</router-link> |

    <span>
      Made with ❤ by
      <a href="https://cmevo.com/" target="_blank">Cmevo Digital</a>
    </span> |
    <router-link :to="{ path: 'privacy-policy' }">Privacy Policy</router-link>
  </footer>
</template>

<style lang="scss">
footer {
  font-size: 1.2rem;
  text-align: center;
  padding: 20px;

  a {
    margin-left: 5px;
    margin-right: 5px;
  }
}
</style>
