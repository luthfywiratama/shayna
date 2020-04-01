<template>
    <div class="shopping">
        <HeaderShayna />
        <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more">
                        <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
                        <span>Shopping Cart</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Shopping Cart Section Begin -->
    <section class="shopping-cart spad">
        <div class="container">
            <div class="row">
                <div class="col-lg-8">
                    <div class="row">
                        <div class="col-lg-12">
                            <div class="cart-table">
                                <table>
                                    <thead>
                                        <tr>
                                            <th>Image</th>
                                            <th class="p-name text-center">Product Name</th>
                                            <th>Price</th>
                                            <th>Action</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="keranjang in keranjangUser" :key="keranjang.id">
                                            <td class="cart-pic first-row">
                                                <img class="photo-item" :src="keranjang.photo" />
                                            </td>
                                            <td class="cart-title first-row text-center">
                                                <h5>{{keranjang.name}}</h5>
                                            </td>
                                            <td class="p-price first-row">${{keranjang.price}}</td>
                                            <td class="delete-item" @click="removeItem(keranjang.id)"><a href="#"><i class="material-icons">
                                              close
                                              </i></a></td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                        <div class="col-lg-8">
                            <h4 class="mb-4 text-left">
                                Informasi Pembeli:
                            </h4>
                            <div class="user-checkout text-left">
                                <form>
                                    <div class="form-group">
                                        <label for="namaLengkap">Nama lengkap</label>
                                        <input type="text" 
                                               class="form-control" 
                                               id="namaLengkap" 
                                               aria-describedby="namaHelp" 
                                               placeholder="Masukan Nama"
                                               v-model="custumerInfo.name"
                                        />
                                    </div>
                                    <div class="form-group">
                                        <label for="namaLengkap">Email Address</label>
                                        <input type="email" 
                                               class="form-control" 
                                               id="emailAddress" 
                                               aria-describedby="emailHelp" 
                                               placeholder="Masukan Email"
                                               v-model="custumerInfo.email"
                                        />
                                    </div>
                                    <div class="form-group">
                                        <label for="namaLengkap">No. HP</label>
                                        <input type="text"
                                               class="form-control" 
                                               id="noHP" 
                                               aria-describedby="noHPHelp" 
                                               placeholder="Masukan No. HP"
                                               v-model="custumerInfo.number">
                                    </div>
                                    <div class="form-group">
                                        <label for="alamatLengkap">Alamat Lengkap</label>
                                        <textarea class="form-control" id="alamatLengkap" rows="3" v-model="custumerInfo.address"></textarea>
                                    </div>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-4">
                    <div class="row">
                        <div class="col-lg-12">
                            <div class="proceed-checkout text-left">
                                <ul>
                                    <li class="subtotal">ID Transaction <span>#SH12000</span></li>
                                    <li class="subtotal mt-3">Subtotal <span>${{totalHarga}}</span></li>
                                    <li class="subtotal mt-3">Pajak <span>10% ${{totalHarga*10/100}}</span></li>
                                    <li class="subtotal mt-3">Total Biaya <span>${{totalBiaya}}</span></li>
                                    <li class="subtotal mt-3">Bank Transfer <span>BCA</span></li>
                                    <li class="subtotal mt-3">No. Rekening <span>777 206 7669</span></li>
                                    <li class="subtotal mt-3">Nama Penerima <span>Luthfy Wiratama Nugraha</span></li>
                                </ul>
                                <!-- <router-link to="/success" class="proceed-btn"> -->
                                    <a @click="checkout()" href="#" class="proceed-btn">I ALREADY PAID</a>
                                <!-- </router-link> -->
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Shopping Cart Section End -->
    </div>
</template>

<script>
import HeaderShayna from '@/components/HeaderShayna.vue'
import axios from 'axios'


export default {
    name: "cart",
    components: {
    HeaderShayna
  },
  data(){
        return{
            products:[],
            keranjangUser:[],
            custumerInfo : {
                name: "",
                email: "",
                number:"",
                address:""

            }
        };
    },
    methods: {
      // saveKeranjang(idProduct,nameProduct, priceProduct, photoProduct) {

      //   var productStored = {
      //     "id": idProduct,
      //     "name": nameProduct,
      //     "price" : priceProduct,
      //     "photo" : photoProduct
      //   }
      //   this.keranjangUser.push(productStored);
      //   const parsed = JSON.stringify(this.keranjangUser);
      //   localStorage.setItem('keranjangUser', parsed);
      // },

      removeItem(xx) {
      // this.keranjangUser.splice(index, 1);
      // const parsed = JSON.stringify(this.keranjangUser);
      // localStorage.setItem("keranjangUser", parsed);
      // window.location.reload();
      let faveGifs = JSON.parse(localStorage.getItem("keranjangUser"));
      let faveGif = faveGifs.map(faveGif => faveGif.id);
      let index = faveGif.findIndex(id => id == xx);
      this.keranjangUser.splice(index, 1);
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem("keranjangUser", parsed);
      window.location.reload();
      // eslint-disable-next-line no-console
      console.log(index);
    },

    checkout() {
        let productIds = this.keranjangUser.map(function(product){
            return product.id
        });

        let checkoutData = {
            'name': this.custumerInfo.name,
            'email': this.custumerInfo.email,
            'number': this.custumerInfo.number,
            'address': this.custumerInfo.address,
            'transaction_total': this.totalBiaya,
            'transaction_status': "PENDING",
            'transaction_details': productIds
        };

         axios
            .post("https://shayna.fitechsolution.com/api/checkout", checkoutData)
            .then(() => this.$router.push('success'))

            .catch(err => console.log(err));
    }

  },
    mounted() {
        if (localStorage.getItem('keranjangUser')) {
            try {
              this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
            } catch(e) {
              localStorage.removeItem('keranjangUser');
            }
          }
    },
    computed: {
    totalHarga() {
        return this.keranjangUser.reduce(function(items, data){
           return +items + +data.price 
        },0); 
    },
    ditambahPajak() {
        return(this.totalHarga *10) /100;
    },
    totalBiaya() {
        return this.totalHarga + this.ditambahPajak;
    }
  }
}
</script>

<style scoped>
    .photo-item {
        width: 80px;
        height: 80px;
    }
</style>