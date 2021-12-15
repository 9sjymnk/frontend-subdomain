<template>
 
        <v-card
    class="mx-auto"
    max-width="344"
    outlined
  >
    <v-list-item three-line>
      <v-list-item-content>
        <div class="text-overline mb-4">
          {{value.type}}
        </div>
        <v-list-item-title class="text-h5 mb-1" >
          {{value.name}}
        </v-list-item-title>
        <v-list-item-subtitle >닭다리 개수는 {{value.chickenlegs}}이고 소스개수는 {{value.sauce}}입니다.</v-list-item-subtitle>
      </v-list-item-content>

      <v-list-item-avatar
        tile
        size="80"
        color="grey"
      ></v-list-item-avatar>
    </v-list-item>

    <v-card-actions>
     
      <v-chip v-if="inTheCart">주문에 담겼습니다</v-chip>
       <v-btn
        v-else
        outlined
        rounded
        text
     
        @click="addToCart"
      >
        주문담기
      </v-btn>
  
      <slot name="buttons"></slot>

    </v-card-actions>
  </v-card>


</template>

<script>
  
  const axios = require('axios').default;   // User에게 할당된 id 값과 함께 요청을 함

  export default {    // 변수, 함수, 클래스 등을 전달하는 명령어
    name: 'Chicken',

    props: {          // props는 property의 약자로, 부모에게서 받아온 데이터이다. 데이터들은 수정이 불가하며 오직 안에 있는 값을 꺼내 사용만 가능하다.
      value: Object,
      editMode: Boolean
    },

    data: ()=>{
      return {
        inTheCart: false
      }
    },

    methods:{

      async addToCart(){

        var cartItem = {
          chicken: this.value._links.self.href,
          customer: "http://localhost:8088/customers/help@uengine.org",
          payment: {
            method: "신용카드",
            status: "결재완료"
          }
        }     // cartItem 정보 추가 (하드하게 추가됨)

        await axios.request({   // async와 await은 세트로 사용되며 async는 특정함수를 비동기로 만들고 
        method: 'POST',         // await은 비동기 작업앞에 쓰이면, 비동기 작업이 결과를 낼 때까지 기달린다.
        url: "/cartItems",      // 즉 여기서 axios는 비동기 통신 라이브러리로 사용된다.
        headers: {'Content-Type': 'application/json'},
        data: cartItem
      });      

       this.inTheCart = true;
     }
    }
  }
</script>
