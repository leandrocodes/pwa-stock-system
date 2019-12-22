<template>
  <div class="home">
    <div class="users-list">
      <vs-row>
        <vs-col style="padding: 2em" vs-type="flex" vs-justify="center" vs-align="center" vs-w="12">
          <vs-button style="margin-right: 2em;" icon="sync" size="large" type="relief" color="#2980b9">Sincronizar</vs-button>
          <vs-button icon="library_add" size="large" type="relief" color="#2980b9">Adicionar</vs-button>
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
                <vs-button @click="edit(uid)" color="warning" type="relief" icon="edit" style="margin-right: 20px;">Editar</vs-button>
                <vs-button @click="confirmar(uid)" color="danger" type="relief" icon="delete">Excluir</vs-button>
              </vs-tr>
            </template>
          </vs-table>
        </vs-col>
      </vs-row>

      <vs-popup class="holamundo" title="Tem certeza?" :active.sync="popupActive">
        <vs-row>
          <vs-col vs-type="flex" vs-justify="center" vs-align="center" vs-w="12">
            <h3>Tem certeza que deseja excluir este cadastro?</h3>
          </vs-col>
          <vs-col style="margin-top: 20px" vs-type="flex" vs-justify="center" vs-align="center" vs-w="12">
            <vs-button @click="deletar(uid)" color="danger" icon="delete_forever">Deletar</vs-button>
          </vs-col>
        </vs-row>
      </vs-popup>
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
  }
}
</script>

<style scoped>
tr {
  margin-top: 1em !important;
}
</style>
