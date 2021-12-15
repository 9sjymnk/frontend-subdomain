<template>
  <v-data-table
    :headers="headers"
    :items="cartItems"
    :items-per-page="5"
    class="elevation-1"
  ></v-data-table>

</template>

<script>

const axios = require('axios').default;

export default {    // public class App{
  name: 'Cart',

  components: {
  },        // Vue 컴포넌트는 Vue의 인스턴스로 볼 수 있다.
            // 따라서 모든 옵션 객체를 사용할 수 있다.
  
  data: () => ({
    headers:[ 
      { text: '이름', value: 'chickenDetail.name' },
      { text: '닭다리 개수', value: 'chickenDetail.chickenlegs' },
      { text: '소스 개수', value: 'chickenDetail.sauce' },
      { text: '결제방법', value: 'payment.method' },
      { text: '결제상태', value: 'payment.status' },
    ],    // 카트 정보 추가 
          // URL:https://vuetifyjs.com/en/components/data-tables/ 참고 

    cartItems: [],
    showCart: false
    }),

  created(){     // 인스턴스가 작성된 후 동기적으로 호출된다.
     this.init();
  },

  methods:{     // template내부에 선언된 methods중에서 update라이프사이클이 동작한다면 함수를 모두 실행한다.
    showCart(){
      this.showCart = true;
    },
  
    async init(){
      const response = await axios.get("/cartItems");
      
      this.cartItems = response.data._embedded.cartItems;

      this.cartItems.forEach(async cartItem => {
        const response = await axios.get(new URL(cartItem._links.chicken.href).pathname);
        cartItem.chickenDetail = response.data;
      })      
    },    // 배열된 것들을 반복하면서 액션을 취한다.
  
  }
};
</script>
