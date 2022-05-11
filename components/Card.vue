<template>
<div class="card mb-3 mt-3">
  <div class="card-body px-5 row">
      <div class="col-md-12">
        <h4>{{ listItem.dates }}</h4>
        <h5>Saldo: {{ listItem.saldo || 0 }}</h5>
        <hr/>
      </div>
      <div class="col-md-6">
        <h5>Pendapatan: Rp. {{ typeof listItem.financeItemInId == 'undefined' || listItem.financeItemInId == '' ? 0 : listItem.financeItemInId.saldo}}</h5>
         <p class="mb-0">Daftar</p>
        <ul>
            <li v-for="(categories,i) in categoryIn"
                :key="i">
              {{categories.description}} : Rp. {{categories.price}} ({{categories.category}})
            </li>
        </ul>
      </div>
      <div class="col-md-6">
        <h5>Pengeluaran: Rp. {{typeof listItem.financeItemOutId == 'undefined' || listItem.financeItemOutId == '' ? 0 : listItem.financeItemOutId.saldo}}</h5>
         <p class="mb-0">Daftar</p>
        <ul>
            <li v-for="(categories,i) in categoryOut"
                :key="i">
              {{categories.description}} : Rp. {{categories.price}} ({{categories.category}} )
            </li>
        </ul>
      </div>
      <div class="col-md-12">
        <div class="action py-2">
           <a class="ms-auto text-decoration-none btn btn-warning fw-bold cursor-pointer"
            v-if="!isCreating" @click="isCreating = !isCreating"
           >+ Tambah Item</a>

            <div class="add-card" v-else>
              <hr>
              <form v-on:submit.prevent="handleSubmit()">
                <div class="card mb-2">
                  <div class="card-header d-flex justify-content-between">
                    <h5 class="my-auto text-warning">Add New Item</h5>
                    <button @click="isCreating = !isCreating" class="btn btn-danger ml-auto d-flex justify-content-end">Cancel</button>
                  </div>
                  <div class="card-body row">
                    <div class="form-group col-md-12 mb-3">
                        <input class="form-control" v-model="form.description" placeholder="Keterangan"/>
                    </div>
                    <div class="form-group col-md-12 mb-3">
                        <select class="form-control" v-model="form.inOut">
                          <option value="pendapatan">Pendapatan</option>
                          <option value="pengeluaran">Pengeluaran</option>
                        </select>
                    </div>
                    <div class="form-group col-md-12 mb-3">
                        <select class="form-control" v-model="form.category">
                          <option value="">Category</option>
                          <option value="Makanan">Makanan</option>
                          <option value="Minuman">Minuman</option>
                          <option value="Transportasi">Transportasi</option>
                          <option value="Gaji">Gaji</option>
                          <option value="Investasi">Investasi</option>
                        </select>
                    </div>
                    <div class="form-group col-md-12 mb-3">
                        <input class="form-control" v-model="form.price" placeholder="Price"/>
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
  </div>
</div>

</template>
<script>
export default {
  name: 'Card',
  props: {
    listItem: {
      type: Object,
      default: {
        dates: '',
        saldo: 0,
        financeItemInId: {
          saldo: 0,
          category: []
        },
        financeItemOutId: {
          saldo: 0,
          category: []
        }
      },
    },
  },
  data() {
    return {
      isCreating:false,
      form: {
        description: '',
        inOut: 'pendapatan',
        category: '',
        price: '',
        id: '',
      },
    }
  },
  computed: {
    categoryIn(){
      if(typeof this.listItem.financeItemInId == 'undefined' || this.listItem.financeItemInId == ''){
        return []
      } else {
        return this.listItem.financeItemInId.category
      }
    },
    categoryOut(){
      if(typeof this.listItem.financeItemOutId == 'undefined' || this.listItem.financeItemOutId == ''){
        return []
      } else {
        return this.listItem.financeItemOutId.category
      }
    }
  },
  methods: {
    handleSubmit: async function(event) {
      if(this.form.description =='' || this.form.inOut =='' || this.form.category =='' || this.form.price ==''){
        alert('Masih ada form yang kosong')
        return;
      }
      this.form.id = this.listItem._id
      await this.$axios.post("finance/created-trans",this.form)
      this.$emit('clicked', 'someValue')
      this.isCreating = !this.isCreating
    }
  }
}
</script>
<style >
.cursor-pointer {
  cursor: pointer;
}
</style>
