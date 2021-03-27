<template>
  <div class="keranjang">
    <Navbar :updateKeranjang="keranjangs"/>

    <div class="container">
      <!-- breadcrumb -->
      <div class="row mt-4">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods" class="text-dark">Food</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Keranjang
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row">
        <div class="col">
          <h2>Keranjang <strong>Saya</strong></h2>
          <div class="table-responsive mt-3">
            <table class="table table-striped">
              <thead>
                <tr>
                  <th scope="col">No</th>
                  <th scope="col">Foto</th>
                  <th scope="col">Makanan</th>
                  <th scope="col">Keterangan</th>
                  <th scope="col">Jumlah</th>
                  <th scope="col">Harga</th>
                  <th scope="col">Total Harga</th>
                  <th scope="col">Hapus</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(keranjang, index) in keranjangs"
                  :key="keranjang.id"
                >
                  <th>{{ index + 1 }}</th>
                  <td>
                    <img
                      :src="'/assets/images/' + keranjang.product.gambar"
                      class="img-fluid shadow"
                      width="200"
                      alt="..."
                    />
                  </td>
                  <td>
                    <strong>{{ keranjang.product.nama }}</strong>
                  </td>
                  <td>
                    {{ keranjang.keterangan ? keranjang.keranjang : "-" }}
                  </td>
                  <td>{{ keranjang.jumlah_pemesanan }}</td>
                  <td align="right">Rp. {{ keranjang.product.harga }}</td>
                  <td align="right">
                    <strong
                      >Rp.
                      {{
                        keranjang.product.harga * keranjang.jumlah_pemesanan
                      }}</strong
                    >
                  </td>
                  <td align="center" class="text-danger">
                    <b-icon-trash
                      @click="hapusKeranjang(keranjang.id)"
                    ></b-icon-trash>
                  </td>
                </tr>

                <tr>
                  <td colspan="6" align="right">
                    <strong>Total harga:</strong>
                  </td>
                  <td align="right">
                    <strong>Rp. {{ totalHarga }}</strong>
                  </td>
                  <td></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "Keranjang",
  components: {
    Navbar,
  },

  data() {
    return {
      keranjangs: [],
    };
  },
  methods: {
    setKerangjangs(data) {
      this.keranjangs = data;
    },
    hapusKeranjang(id) {
      axios
        .delete("http://localhost:3000/keranjangs/" + id)
        //handle success
        .then(() => {
          this.$toast.error("Sukses Hapus Keranjang", {
            type: "error",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });
          //update data keranjang
          axios
            .get("http://localhost:3000/keranjangs")
            //handle success
            .then((response) => this.setKerangjangs(response.data))
            //handle error
            .catch((error) => console.log(error));
        })
        //handle error
        .catch((error) => console.log(error));
    },
  },

  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      //handle success
      .then((response) => this.setKerangjangs(response.data))
      //handle error
      .catch((error) => console.log(error));
  },

  computed: {
    totalHarga() {
      return this.keranjangs.reduce(function (items, data) {
        return items + data.product.harga * data.jumlah_pemesanan;
      }, 0);
    },
  },
};
</script>

<style>
</style>