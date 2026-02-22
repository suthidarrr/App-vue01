<template>
  <div class="container my-5">
    <h2 class="text-center mb-4">รายการสินค้า</h2>

    <div v-if="loading" class="text-center">กำลังโหลดข้อมูล...</div>
    <div v-else-if="error" class="alert alert-danger">{{ error }}</div>

    <div v-else class="row">
      <div class="col-md-3" v-for="data in Alldata" :key="data.product_id">

        <div class="card shadow-sm mb-4 h-100">
          <img
            :src="'http://localhost/App-vue01/php_api/uploads/' + data.image"
            class="card-img-top"
            style="height: 250px; object-fit: cover;"
            :alt="data.product_name"
          >

          <div class="card-body text-center d-flex flex-column">
            <h5 class="card-title">{{ data.product_name }}</h5>
            <p class="card-text">{{ data.price }} บาท</p>
            
            <div class="mt-auto card-footer bg-white border-0">
              <router-link 
                :to="'/productDetail?id=' + data.product_id"
                class="btn btn-sm btn-outline-primary w-100 mb-2"
              >
                ดูรายละเอียด
              </router-link>

              <button class="btn btn-sm btn-primary w-100">
                เพิ่มลงตะกร้า
              </button>
            </div>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  name: "DataList",
  setup() {
    const Alldata = ref([]);
    const loading = ref(true);
    const error = ref(null);

    const fetchData = async () => {
      try {
        const response = await fetch("http://localhost/App-vue01/php_api/show_product.php");
        if (!response.ok) {
          throw new Error("ไม่สามารถดึงข้อมูลได้");
        }
        const result = await response.json();
        
        // ✅ ตรวจสอบข้อมูลที่ได้จาก PHP ใน Console (F12)
        console.log("Data from PHP:", result);
        
        Alldata.value = result; 
      } catch (err) {
        error.value = err.message;
        console.error("Fetch error:", err);
      } finally {
        loading.value = false;
      }
    };

    onMounted(() => {
      fetchData();
    });

    return {
      Alldata,
      loading,
      error
    };
  }
};


</script>