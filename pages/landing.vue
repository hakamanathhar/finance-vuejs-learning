<template>
<div class="container">
  <div class="d-flex mt-5">
    <button @click="isCreateTrans = !isCreateTrans" class="nav-link text-black btn btn-info ms-auto">➕ Tambah Transaksi</button>
  </div>
  <div v-if="isCreateTrans">
    <div class="card mb-3 mt-3">
      <div class="card-body px-5 row">
          <form v-on:submit.prevent="handleSubmit()">
            <div class="card mb-2">
              <div class="card-header d-flex justify-content-between">
                <h5 class="my-auto text-warning">Tambah Transaksi</h5>
                <button @click="isCreateTrans = !isCreateTrans" class="btn btn-danger ml-auto d-flex justify-content-end">Cancel</button>
              </div>
              <div class="card-body row">
                  <div class="form-group col-md-12 mb-3">
                      <input class="form-control text-black bg-white" type="date" v-model="formTrans.date" placeholder="Keterangan"/>
                  </div>
                  <div class="col-md-12">
                      <button type="submit" class="btn btn-info mt-2 w-100">Submit</button>
                    </div>
              </div>
            </div>
          </form>
      </div>
    </div>
  </div>
  <Card
     v-for="(listItem) in resultQuery"
    :key="listItem.id"
    :listItem="listItem"
    @clicked="onClickChild"
  />
</div>

</template>
<script>
import Card from '~/components/Card'
export default {
  name: 'Landing',
  components: {
    Card
  },
  async created() {
    const res = await this.$axios.get("finance/list")
    this.listItem = res.data.data
  },
  data() {
    return {
      isCreateTrans:false,
      searchQuery: '',
      searchCategory: 'all',
      orderData: 'none',
      formTrans: {
        date: ''
      },
      listItem: [

      ]
    }
  },
  computed: {
    resultQuery() {
      if (this.searchQuery) {
        return this.listItem.filter((item) => {
          return this.searchQuery
            .toLowerCase()
            .split(" ")
            .every((v) => item.title.toLowerCase().includes(v));
        });
      } else if(this.searchCategory && this.searchCategory != 'all') {
        return this.listItem.filter((item) => {
          return this.searchCategory
            .toLowerCase()
            .includes(item.category.toLowerCase())
        });
      } else {
        return this.order(this.orderData)
      }
    },
  },
  methods: {
    async onClickChild (value) {
      const res = await this.$axios.get("finance/list")
      this.listItem = res.data.data
    },
    order(value) {
        if(value === 'asc')
          return this.listItem.sort(function(a,b){
            if (a.title < b.title)
              return -1;
            if (a.title > b.title)
              return 1;
            return 0;
          });
        else if(value === 'desc')
          return this.listItem.sort(function(a,b){
            if (a.title < b.title)
              return 1;
            if (a.title > b.title)
              return -1;
            return 0;
          });
        else
          return this.listItem
    },
    async handleSubmit() {
      await this.$axios.post("finance/add-item",this.formTrans)
      this.onClickChild("")
      this.isCreateTrans = !this.isCreateTrans
    }
  }
}
</script>
