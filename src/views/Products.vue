<template>
  <div class="home">
    <div class="users-list">
      <vs-row>
        <vs-col style="padding: 2em" vs-type="flex" vs-justify="center" vs-align="center" vs-w="12">
          <vs-button style="margin-right: 2em;" icon="sync" size="large" type="relief" color="#66BB6A" @click='sync'>Sincronizar</vs-button>
          <vs-button icon="library_add" size="large" type="relief" color="#66BB6A">Adicionar</vs-button>
        </vs-col>
        <vs-col style="padding-bottom: 1.5em" vs-type="flex" vs-justify="center" vs-align="center" vs-w="12">
          <h2>Listagem de Produtos</h2>
        </vs-col>
      </vs-row>

      <vs-row class="list-all-users">
        <vs-col vs-type="flex" vs-justify="center" vs-align="center" vs-w="12">
          <vs-table :data="products">
            <template slot="thead">
              <vs-th><h3>Nome</h3></vs-th>
              <vs-th><h3>Marca</h3></vs-th>
              <vs-th><h3>Preço</h3></vs-th>
              <vs-th><h3>Quantidade</h3></vs-th>
            </template>

            <template slot-scope="{ data }">
              <vs-tr :data="tr" :key="tr.id" v-for="tr in data">
                <vs-td :data="tr.data.name">{{ tr.data.name }}</vs-td>
                <vs-td :data="tr.data.brand">{{ tr.data.brand }}</vs-td>
                <vs-td :data="tr.data.price">{{ tr.data.price }}</vs-td>
                <vs-td :data="tr.data.price">{{ tr.data.quantity }}</vs-td>
                <br />
                <vs-button @click="edit(uid)" color="#f39c12" type="flat" icon="edit" style="margin-right: 20px;">Editar</vs-button>
                <vs-button @click="confirmar(uid)" color="#c0392b" type="flat" icon="delete">Excluir</vs-button>
              </vs-tr>
            </template>
          </vs-table>
        </vs-col>
      </vs-row>

    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: ''
    }
  },
  created() {
    this.axios
      .get('/products')
      .then(res => {
        this.products = res.data //console.log(res)
      })
      .catch(err => {
        this.products = err
      })
  },
  methods: {
    sync() {
      this.$router.go('/')
    }
  }
}
</script>

<style scoped></style>
