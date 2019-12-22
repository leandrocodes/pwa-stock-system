<template>
  <div class="home">
    <div class="nav" style="padding: 2em">
      <b-button type="is-info" icon-left="sync" style="margin-right: 2em">Sincronizar</b-button>
      <b-button type="is-info" icon-left="package-variant">Adicionar</b-button>
      <h1 class="title" style="margin-top: 1em;">Listagem de Produtos</h1>
    </div>
    <div class="columns" style="padding: 1em 2em;">
      <div class="column is-6 is-offset-3">
        <b-table :data="products" stripped hoverable mobile-cards>
          <template slot-scope="product">
            <b-table-column field="name" label="Nome">
              {{ product.row.data.name }}
            </b-table-column>

            <b-table-column field="brand" label="Maca">
              {{ product.row.data.brand }}
            </b-table-column>

            <b-table-column field="quantity" label="Quantidade">
              {{ product.row.data.quantity }}
            </b-table-column>

            <b-table-column field="price" label="Preço">
              {{ product.row.data.price }}
            </b-table-column>
            <b-table-column>
              <b-button type="is-warning" icon-left="pencil-outline" style="margin-right: 2em">Editar</b-button>
              <b-button type="is-danger" icon-left="trash-can-outline"> Deletar </b-button>
            </b-table-column>
          </template>
        </b-table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [],
      popUpAdd: false,
      product: {
        name: '',
        brand: '',
        price: '',
        quantity: ''
      }
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
