<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Products</router-link> |
      <router-link to="/about">About</router-link>
      <div>
        {{ result }}
        <div>
          <span>
            <vs-icon icon="mood"></vs-icon>
          </span>
          <span>
            <vs-icon icon="child_care" size="small" color="red"></vs-icon>
          </span>
          <span>
            <vs-icon icon="pets" size="medium" color="primary"></vs-icon>
          </span>
          <span>
            <vs-icon
              icon="android"
              size="large"
              color="rgb(70, 150, 0)"
            ></vs-icon>
          </span>
          <span>
            <vs-icon icon="polymer" size="75px" color="#BC1254"></vs-icon>
          </span>
        </div>
      </div>
    </div>
    <router-view />
  </div>
</template>

<script>
export default {
  data() {
    return {
      result: ""
    };
  },
  created() {
    this.axios
      .get("/procuts")
      .then(res => {
        this.result = res; //console.log(res)
      })
      .catch(err => {
        this.result = err;
      });
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
