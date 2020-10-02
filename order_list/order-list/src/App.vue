<template>
  <div id="app">
    <div class="container">
      <div class="columns mt-6">
        <div class="column">
          <h1 class="title">shopping cart</h1>
          <div class="field">
            <label class="label">Name</label>
            <div class="control">
              <input v-model="product.name" class="input" type="text" placeholder="Text input" />
            </div>
          </div>
          <div class="field">
            <label class="label">Categories</label>
            <div class="control">
              <div class="select is-fullwidth">
                 <select  v-model="product.category">
                <option v-for="(item,index) in categories" :value="item" :key="index">{{ item }}</option>
                </select>
              </div>
            </div>
          </div>
          <div class="field">
            <label class="label">price</label>
            <div class="control">
              <input v-model.number="product.price" class="input" type="text" placeholder="Text input" />
            </div>
          </div>
          <div class="field">
            <label class="label">image</label>
            <div class="control">
              <input v-model="product.image" class="input" type="text" placeholder="Text input" />
            </div>
          </div>
          <div class="field">
            <label class="label">description</label>
            <div class="control">
              <textarea v-model="product.description" class="textarea" placeholder="Textarea"></textarea>
            </div>
          </div>
          <div class="field is-grouped">
            <div class="control">
              <button v-if="editMode" @click="updateProduct()" class="button is-link is-success">done</button>
              <button v-else @click="addProduct" class="button is-link">Submit</button>
            </div>
          </div>
        </div>
        <div class="column">
          <h2 class="title">total:{{ total }}</h2>
            <select v-model="selectedCategory">
              <option v-for="(item,index) in categories" :value="item" :key="index">
                {{ item }}
              </option>
            </select>

          <div class="card mt-3" v-for="(item, index) in productsFiltered" :key="index">
            <div class="card-content">
              <div class="media">
                <div class="media-left">
                  <figure class="image is-48x48">
                    <img 
                      :src="item.image"
                      alt="Placeholder image"
                    />
                  </figure>
                </div>
                <div class="media-content">
                  <p class="title is-4">{{ item.name }}</p>
                  <p class="stitle is-4">{{ item.category }}</p>
                  <p class="subtitle is-6">{{ item.price }}</p>
                </div>
                <footer class="card-footer">
                  <a @click="editProduct(item)" href="#" class="card-footer-item button is-info">Edit</a>
                  <a @click="deleteProduct(index)" href="#" class="card-footer-item button is-danger">Delete</a>
                </footer>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>




<script>
export default {
  data() {
    return {
      categories: ["frutes","vegetables","meat","fish"],
      selectedCategory: " ",
      product: {
        name: " ",
        price: " ",
        category: " ",
        image: " ",
        description: " ",
      },
      product_list: [],
      editMode: false,
    };
  },
  methods: {
    addProduct() {
      const p = this.product
      if(p.name === " " || p.price === " " || p.image === " " || p.description === " " || p.category === " "){
        alert("all fields have to be added")
        return 0;
      }
      const myProduct = {...this.product}
      this.product_list.push(myProduct)
      this.product = {}
      this.clearProduct()
    },
    deleteProduct(index) {
      this.product_list.splice(index,1)
    },
    clearProduct() {
      this.product = {
        name: " ",
        price: " ",
        image: " ",
        description: " "
      }
    },
    editProduct(item) {
      this.product = item
      this.editMode = true;
    },
    updateProduct() {
      this.editMode = false;
      this.clearProduct()
    }
  },
  computed: {
    total() {
      if(this.product_list.length === 0 ) return 0;
       const price_list = this.product_list.map(item => item.price)
       return price_list.reduce((acc,price) => acc + price)
    },
    productsFiltered(){
      if(this.selectedCategory === " ") return this.product_list;
      return this.product_list.filter( item => item.category === this.selectedCategory)
    }
  }
};
</script>


