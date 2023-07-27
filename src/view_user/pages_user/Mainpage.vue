<template>
  <v-container rounded-0 fluid style="margin-top: 42px">
    <userHeader :dialog="openAddress" @chosenAddress="openDialog = $event" />
    <Card_User_Order
      style="z-index: 1000; display: none"
      :currentID="currentID"
      :dialog="openDialog"
      :id="currentID"
      :image_url="product.image_url"
      :name="product.name"
      :description="product.description"
      :price="product.price"
      :isInProductListing="0"
    >
    </Card_User_Order>
    <div>
      <template style="board">
        <v-carousel cycle hide-delimiter-background show-arrows>
          <v-carousel-item v-for="(url_header, i) in urls_header" :key="i">
            <div>
              <a href="#">
                <img :src="url_header" alt="" style="width: 100%" />
              </a>
            </div>
          </v-carousel-item>
        </v-carousel>
      </template>
    </div>
    <productListing :dialog="false" :currentID="currentID" />
    <div></div>
    <!-- <mainHeader/> -->
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  name: "Mainpage",
  //    components: {
  //     mainHeader: () => import('@/layouts/Header/mainHeader'),
  //   },
  data() {
    return {
      openAddress: true,
      currentID: "0",
      openDialog: false,
      product: {},
      topping_items: [],
      product_relations: [],
      urls_header: [
        "https://file.hstatic.net/1000075078/file/banner_web_moi_-_desktop-2_5ac1ead0587b483bb6cb4a61e056eba0.jpg",
        "https://file.hstatic.net/1000075078/file/banner_web_moi_-_desktop-3_0d7376e039b7422e97cf3feda28c4fbf.jpg",
        "https://file.hstatic.net/1000075078/file/web_desktop_6239afdcc8a3423c99a489210f67e44c.jpg",
        "https://file.hstatic.net/1000075078/file/banner_web_moi_-_desktop_262c12a949d2433c81a8adfb589e6c7d.jpg",
        "https://file.hstatic.net/1000075078/file/banner_web_moi_-_desktop-4_2fc4c530b0544c739eb8b6693f4f78ff.jpg",
      ],
    };
  },
  created() {
    this.currentID = this.$route.path.slice(6);
    this.getProductByID();
    if (this.openDialog == true) {
      this.openAddress = false;
    }
    // setTimeout(()=>{}, 5000)
  },

  methods: {
    getProductByID() {
      axios
        .post("http://127.0.0.1:8000/api/admin/product/getProductInfo", {
          product_id: this.currentID,
        })
        .then((response) => {
          console.log("START res product\n");
          console.log(response);
          console.log("END\n");
          this.product = response.data.product;
          // console.log("Product 1 : ",this.product)
          // this.product.id = this.currentID
          // this.topping_items = response.data.toppings
          // this.product_relations = response.data.same
          // for(let index in this.topping_items){
          //     let topping_item = this.topping_items[index]
          //     topping_item.count = 0
          //     // topping_item.id = index + 1

          // }

          console.log("Product: ", this.product);
          // console.log("ITEMS in function:")
          // console.log(this.items)
        })
        .catch((error) => {
          console.log("CAN NOT");
          console.log(error.response);
        });
    },
  },

  watch: {
    openDialog() {
      console.log("Change: ", this.openDialog);
      if (this.openDialog == true) {
        this.openAddress = false;
      }
    },
  },
  components: {
    userHeader: () => import("@/layouts/Header/userHeader"),
    productListing: () => import("@/components/userComponents/productListing"),
    Card_User_Order: () => import("@/components/userComponents/Card_User_Order"),
  },

};
</script>

<style scoped>
.board {
  background-color: orange;
  margin-top: -70px;
  margin-left: -24px;
  margin-right: -24px;
  padding-top: 20px;
  padding-left: 160px;
  padding-bottom: 0px;
  padding-right: 160px;
  margin-bottom: 0px;
}

.fill-height {
  align-items: start !important;
  margin-top: 42px;
}
</style>
