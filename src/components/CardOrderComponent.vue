<template>
    <b-container class="bv-example-row card shadow py-2 px-5 card-product">
        <b-row>
            <b-col>{{order.created_at}}</b-col>
            <b-col align="right"> Status: <strong>{{ order.status }}</strong></b-col>
        </b-row>

        <b-row class="mt-5">
            <b-col><img :src=" 'assets/images/' + order.order_items[0].product.picture " class="card-img-order" alt="..." /></b-col>
            <b-col class="border-left my-auto">
                <div v-for="order_item in order.order_items" :key="order_item.id">
                    <p>{{ order_item.product.name }}</p>
                </div>
            </b-col>
            <b-col class="mx-auto my-auto">
                <p>Total Price:</p>
                <p><strong>{{order.total_after_discount}}</strong></p>
                
            </b-col>
        </b-row>
        <b-row class="mt-3" align="right">
            <b-col>

            </b-col>
            <b-col>
                <router-link class="btn btn-success float-right" :to="'/orders/'+order.id"> Detail</router-link>
            </b-col>
        </b-row>
        
    </b-container>
</template>
  
<script>
  export default {
    name: "CardOrder",
    props: ["order"],
  };
</script>
  
<style>

</style>