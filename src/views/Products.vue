<template>
  <div class="home">
    <div class="nav" style="padding: 2em">
      <!-- <b-button type="is-link" icon-left="sync" style="margin-right: 2em" @click="sync" :loading="syncBtn">Sincronizar</b-button> -->
      <b-button type="is-link" icon-left="package-variant" @click="addModal">Adicionar produto</b-button>
      <h1 class="title" style="margin-top: 1em;">Listagem de Produtos</h1>
    </div>
    <div class="columns is-centered" style="padding: 1em 2em;">
      <div class="column is-8">
        <b-table :data="products" stripped hoverable mobile-cards>
          <template slot-scope="product">
            <b-table-column field="name" label="Nome do produto:">
              {{ product.row.data.name }}
            </b-table-column>

            <b-table-column field="brand" label="Marca:">
              {{ product.row.data.brand }}
            </b-table-column>

            <b-table-column field="quantity" label="Quantidade:">
              {{ product.row.data.quantity }}
            </b-table-column>

            <b-table-column field="price" label="Preço:"> R$ {{ product.row.data.price }} </b-table-column>
            <b-table-column>
              <b-button type="is-warning" icon-left="pencil-outline" style="margin-right: 2em" @click="editModal(product.row.id)"> Editar</b-button>
              <b-button type="is-danger" icon-left="trash-can-outline" @click="delModal(product.row.id)"> Deletar </b-button>
            </b-table-column>
          </template>
        </b-table>
      </div>
    </div>
    <b-modal :active.sync="modalAdd" has-modal-card trap-focus aria-role="dialog" aria-modal>
      <div class="card" style="padding: 1em 2em; border-radius: .5em;">
        <h4 class="title is-size-4 has-text-info">Adicionar produto</h4>
        <div class="card-content">
          <b-field label="Nome do produto">
            <b-input placeholder="Calça Jeans" v-model="product.name"></b-input>
          </b-field>
          <b-field label="Marca do produto">
            <b-input placeholder="Chilli Beans" v-model="product.brand"></b-input>
          </b-field>
          <b-field label="Quantidade em estoque">
            <b-input placeholder="100" v-model="product.quantity"></b-input>
          </b-field>
          <b-field label="Preço do produto">
            <b-input placeholder="R$ 199" v-model="product.price"></b-input>
          </b-field>
          <b-button style="margin-right: 2em; margin-top: 1em;" type="is-danger" icon-left="close-circle" @click="modalAdd = false">
            Cancelar
          </b-button>
          <b-button style="margin-top: 1em;" type="is-success" icon-left="plus-circle" @click="addProduct" :loading="addBtn"> Adicionar </b-button>
        </div>
      </div>
    </b-modal>

    <b-modal :active.sync="modalEdit" has-modal-card trap-focus aria-role="dialog" aria-modal>
      <div class="card" style="padding: 1em; border-radius: .5em;">
        <div class="card-content" v-if="editLoadModal === false">
          <h4 class="title is-size-4 has-text-info">Editar produto</h4>
          <b-field label="Nome do produto">
            <b-input placeholder="Calça Jeans" :value="product.name" v-model="product.name"></b-input>
          </b-field>
          <b-field label="Marca do produto">
            <b-input placeholder="Chilli Beans" :value="product.brand" v-model="product.brand"></b-input>
          </b-field>
          <b-field label="Quantidade em estoque">
            <b-input placeholder="100" :value="product.quantity" v-model="product.quantity"></b-input>
          </b-field>
          <b-field label="Preço do produto">
            <b-input placeholder="R$ 199" :value="product.price" v-model="product.price"></b-input>
          </b-field>
          <b-button style="margin-right: 2em; margin-top: 1em;" type="is-info" outlined icon-left="close-circle" @click="modalEdit = false">
            Cancelar
          </b-button>
          <b-button style="margin-top: 1em;" type="is-warning" icon-left="pencil-outline" @click="editProduct" :loading="editBtn"> Editar </b-button>
        </div>
        <div v-else>
          <b-notification :closable="false" style="padding: 3em;">
            <b-loading :is-full-page="false" :active.sync="editLoadModal" :can-cancel="true"></b-loading>
          </b-notification>
        </div>
      </div>
    </b-modal>

    <b-modal :active.sync="modalDel" has-modal-card trap-focus aria-role="dialog" aria-modal>
      <div class="card" style="padding: 1em 2em; border-radius: .5em;">
        <div class="card-content" v-if="delLoadModal === false">
          <h4 class="title is-size-4 has-text-dark">Deseja realmente deletar o produto {{ product.name }} {{ product.brand }}?</h4>
          <b-button style="margin-right: 2em;" type="is-info" outlined icon-left="close-circle" @click="modalDel = false">
            Cancelar
          </b-button>
          <b-button type="is-danger" icon-left="trash-can-outline" @click="delProduct()" :loading="delBtn"> Deletar </b-button>
        </div>
        <div v-else>
          <b-notification :closable="false" style="padding: 3em;">
            <b-loading :is-full-page="false" :active.sync="delLoadModal" :can-cancel="true"></b-loading>
          </b-notification>
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
      syncBtn: false,
      addBtn: false,
      editBtn: false,
      delBtn: false,
      editLoadModal: true,
      delLoadModal: true,
      productId: '',
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
      this.syncBtn = true
      this.axios
        .get('/products')
        .then(res => {
          this.products = res.data //console.log(res)
        })
        .catch(err => {
          this.products = err
        })
        .finally(() => {
          this.syncBtn = false
        })
    },
    addModal() {
      this.modalAdd = true
      this.clear()
    },
    addProduct() {
      this.addBtn = true
      this.axios
        .post('/products', this.product)
        .then(() => {
          this.sync()
        })
        .finally(() => {
          this.clear()
          this.addBtn = false
          this.modalAdd = false
          this.$buefy.toast.open({
            message: 'Produto adicionado!',
            type: 'is-success',
            position: 'is-bottom'
          })
        })
    },
    editModal(id) {
      this.editLoadModal = true
      this.modalEdit = true
      this.axios
        .get('/products/' + id)
        .then(res => {
          this.product = res.data.data
          this.productId = id
        })
        .finally(() => {
          this.editLoadModal = false
        })
    },
    delModal(id) {
      this.delLoadModal = true
      this.modalDel = true
      this.axios
        .get('/products/' + id)
        .then(res => {
          this.product = res.data.data
          this.productId = id
        })
        .finally(() => {
          this.delLoadModal = false
        })
    },
    delProduct() {
      this.delBtn = true
      this.axios
        .delete('/products/' + this.productId)
        .then(() => {
          this.modalDel = false
          this.sync()
        })
        .finally(() => {
          this.clear()
          this.$buefy.toast.open({
            message: 'Produto deletado!',
            type: 'is-danger',
            position: 'is-bottom'
          })
          this.delBtn = false
        })
    },
    editProduct() {
      this.editBtn = true
      this.axios
        .patch('/products/' + this.productId, this.product)
        .then(() => {
          this.modalEdit = false
          this.sync()
        })
        .finally(() => {
          this.clear()
          this.$buefy.toast.open({
            message: 'Produto editado!',
            type: 'is-info',
            position: 'is-bottom'
          })
          this.editBtn = false
        })
    },
    clear() {
      this.product.name = ''
      this.product.brand = ''
      this.product.quantity = ''
      this.product.price = ''
      this.productId = ''
    }
  }
}
</script>

<style scoped></style>
