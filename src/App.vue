<template>
  <v-app id="inspire">
    <v-app-bar
      app
      color="white"
      flat
    >
      <v-container class="py-0 fill-height">
        <v-avatar
          class="mr-10"
          color="grey darken-1"
          size="32"
        ></v-avatar>

        <v-btn
          v-for="link in links"
          :key="link.href"
          :href="link.href"
          text
        >
          {{ link.title }}
        </v-btn>

        <v-spacer></v-spacer>

        <v-responsive max-width="260">
          <v-text-field
            dense
            flat
            hide-details
            rounded
            solo-inverted
          ></v-text-field>
        </v-responsive>
      </v-container>
    </v-app-bar>

    <v-main class="grey lighten-3">
      <v-container>
        <v-row>
          <v-col cols="2">
            <v-sheet rounded="lg">
              <v-list color="transparent">
                <v-list-item @click="showCart = true">
                  <v-list-item-content>
                    <v-list-item-title>
                      주문보기
                    </v-list-item-title>
                  </v-list-item-content>
                </v-list-item>

                <v-divider class="my-2"></v-divider>

                <v-list-item
                  link
                  color="grey lighten-4"
                >
                  <v-list-item-content>
                    <v-list-item-title>
                      Refresh
                    </v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </v-list>
            </v-sheet>
          </v-col>

          <v-col>
            <v-sheet
              min-height="70vh"
              rounded="lg"
            >
              <Cart v-if="showCart"></Cart>
             
              <Chicken v-else v-model="chickens[index]" v-for="(chicken, index) in chickens" :key="index"></Chicken>


            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>

import Chicken from './components/Chicken'    // import components.Chicken;
import Cart from './components/Cart'    // import components.Chicken;

const axios = require('axios').default;

export default {    // public class App{
  name: 'App',      // 변수, 함수, 클래스 등을 전달하는 명령어

  components: {
    Chicken, Cart
  },
  
  data: () => ({
      links: [
        {title: '메뉴 확인', href:"/"},
        
      ],
      items: [
        '후라이드 치킨',
        '양념 치킨'
      ],
      chickens: [
    
       
      ],
      showCart: false
    }),

  created(){      // 인스턴스가 작성된 후 동기적으로 호출된다.
     this.init();
  },

  methods:{     // template내부에 선언된 methods중에서 update라이프사이클이 동작한다면 함수를 모두 실행한다.
    showCart(){
      this.showCart = true;
    },
  
    async init(){     
      const response = await axios.get("/seasonedspicychickens");
      
      this.chickens = response.data._embedded.seasonedspicychickens;
    },             // async와 await은 세트로 사용되며 async는 특정함수를 비동기로 만들고 
                   // await은 비동기 작업앞에 쓰이면, 비동기 작업이 결과를 낼 때까지 기달린다.
  
  }
};
</script>
