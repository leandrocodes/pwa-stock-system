<template>
  <div class="home">
    <div class="nav" style="padding: 2em">
      <b-button type="is-info" icon-left="sync" style="margin-right: 2em" @click="sync">Sincronizar</b-button>
      <b-button type="is-info" icon-left="package-variant" @click="modalAdd = true">Adicionar</b-button>
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
              <b-button type="is-warning" icon-left="pencil-outline" style="margin-right: 2em" @click="editModal(product.row.id)"> Editar</b-button>
              <b-button type="is-danger" icon-left="trash-can-outline" @click="delModal(product.row.id)"> Deletar </b-button>
            </b-table-column>
          </template>
        </b-table>
      </div>
    </div>
    <b-modal :active.sync="modalAdd" has-modal-card trap-focus aria-role="dialog" aria-modal>
      <div class="card">
        <div class="card-content">
          <b-field label="Nome do produto">
            <b-input placeholder="Calça Jeans"></b-input>
          </b-field>
          <b-field label="Marca do produto">
            <b-input placeholder="Chilli Beans"></b-input>
          </b-field>
          <b-field label="Quantidade em estoque">
            <b-input placeholder="100"></b-input>
          </b-field>
          <b-field label="Preço do produto">
            <b-input placeholder="R$ 199"></b-input>
          </b-field>
          <b-button style="margin-right: 2em; margin-top: 1em;" type="is-danger" icon-left="close-circle" @click="modalAdd = false">
            Cancelar
          </b-button>
          <b-button style="margin-top: 1em;" type="is-success" icon-left="plus-circle"> Adicionar </b-button>
        </div>
      </div>
    </b-modal>

    <b-modal :active.sync="modalEdit" has-modal-card trap-focus aria-role="dialog" aria-modal>
      <div class="card" style="padding: 1em 2em; border-radius: .5em;">
        <h4 class="title is-size-4 has-text-info">Editar produto</h4>
        <div class="card-content">
          <b-field label="Nome do produto">
            <b-input placeholder="Calça Jeans" :value="product.name"></b-input>
          </b-field>
          <b-field label="Marca do produto">
            <b-input placeholder="Chilli Beans" :value="product.brand"></b-input>
          </b-field>
          <b-field label="Quantidade em estoque">
            <b-input placeholder="100" :value="product.quantity"></b-input>
          </b-field>
          <b-field label="Preço do produto">
            <b-input placeholder="R$ 199" :value="product.price"></b-input>
          </b-field>
          <b-button style="margin-right: 2em; margin-top: 1em;" type="is-danger" icon-left="close-circle" @click="modalEdit = false">
            Cancelar
          </b-button>
          <b-button style="margin-top: 1em;" type="is-success" icon-left="plus-circle"> Adicionar </b-button>
        </div>
      </div>
    </b-modal>

    <b-modal :active.sync="modalDel" has-modal-card trap-focus aria-role="dialog" aria-modal>
      <div class="card" style="padding: 1em 2em; border-radius: .5em;">
        <h4 class="title is-size-4 has-text-dark">Deseja realmente deletar o produto {{ product.name }} ?</h4>
        <div class="card-content">
          <b-button style="margin-right: 2em;" type="is-info" outlined icon-left="close-circle" @click="modalDel = false">
            Cancelar
          </b-button>
          <b-button type="is-danger" icon-left="trash-can-outline"> Deletar </b-button>
        </div>
      </div>
    </b-modal>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [],
      modalAdd: false,
      modalEdit: false,
      modalDel: false,
      product: {
        id: '',
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
    },
    editModal(id) {
      /* eslint-disable no-console */
      console.log(id)
      this.modalEdit = true
      this.axios.get('/products/' + id).then(res => {
        this.product = res.data.data
        this.product.id = id
      })
    },
    delModal(id) {
      /* eslint-disable no-console */
      console.log(id)
      this.modalDel = true
      this.axios.get('/products/' + id).then(res => {
        this.product = res.data.data
        this.product.id = id
      })
    }
  }
}
</script>

<style scoped></style>
