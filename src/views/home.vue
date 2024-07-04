<script setup>
import { ref, onMounted, watch, computed } from 'vue'
import 'vue3-carousel/dist/carousel.css'
import { Carousel, Slide, Pagination, Navigation } from 'vue3-carousel'
import { getAssetsFile } from '@/utils/commonUse'
components: {
  Carousel,
  Slide,
  Pagination,
  Navigation
}
const showBigPhoto = ref(false)
const p = ref('')
const pType = ref('')
const pLimit = ref(0)
const nowIndex = ref(0)
const nowPic = computed(() => {
  return getAssetsFile(`${p.value}.png`)
})
const show = (type, idx, limit) => {
  pLimit.value = limit
  pType.value = type
  showBigPhoto.value = true
  nowIndex.value = idx
  p.value = type + idx.toString()
  const elHtml = document.querySelector('html')
  elHtml.style.overflowY = 'hidden'
}
const breakpoints = {
  // 700px and up
  700: {
    itemsToShow: 3,
    snapAlign: 'center',
  },
  // 1024 and up
  1290: {
    itemsToShow: 5,
    snapAlign: 'start',
  },
}
const changePicIndex = (type) => {
  type == 'back' ? nowIndex.value -= 1 : nowIndex.value += 1
  let n = nowIndex.value
  p.value = pType.value + n.toString()
}
import { getAssetsFileIcon } from '@/utils/commonUse.js'
const optionList = ref(['HOME', 'SERVICES', 'GALLERY', 'CONTACT US'])
const iconList = ref(['NAILS', 'WAXING', 'EYELASH', 'FACIAL', 'BACK & FEET RELAXING'])
const menuShow = ref(false)

const scrollTo = (id) => {
  if(menuShow.value){
    menuShow.value = false
  }
  const element = document.getElementById(id)
  if(element){
    element.scrollIntoView({behavior: 'smooth', block: 'start'})
  }
}
const screenWidth = ref(window.innerWidth);
const handleResize = () => {
  screenWidth.value = window.innerWidth;
  if(screenWidth.value <= 1024){
    isComputer.value = false
  } else {
    isComputer.value = true
  }
};
const isComputer = ref(true)
watch(screenWidth, (newValue, oldValue) => {
  if(newValue <= 1025){
    isComputer.value = false
  } else {
    isComputer.value = true
  }
});
onMounted(() => {
  window.addEventListener('resize', handleResize);
  handleResize()
});
const closeBigPhoto = () => {
  showBigPhoto.value = false
  const elHtml = document.querySelector('html')
  elHtml.style.overflowY = null
}
</script>

<template>
  <div class="wrapper">
    <div id="HOME" class="header" :class="{'isPhone': !isComputer}">
      <div class="section1" :class="{'phoneFixed': !isComputer}">
        <div class="section1Title">Central Nail Spa 2022</div>
        <div class="optionList">
          <div v-for="(item, index) in optionList" :key="index" @click="scrollTo(item)">{{ item }}</div>
        </div>
        <div @click="menuShow = true" class="menuIcon"><img src="../assets/icon/menuIcon.svg" alt=""></div>
        <el-drawer v-model="menuShow" :with-header="false" size="45%">
          <div style="cursor: pointer;margin: 20px 0;" v-for="(item, index) in optionList" :key="index" @click="scrollTo(item)">{{ item }}</div>
        </el-drawer>
      </div> 
      <div class="section2">Polish your life <hr>with perfect nails!</div>
      <div class="section3"><img src="../assets/img/1.jpg" alt="" class="object-cover"></div>
      <div class="paintWall" />
    </div>
    <!-- 輪播 -->
    <div class="w-screen py-10 flex flex-col items-center tablet:hidden">
      <div class="max-w-[1200px]">
        <Carousel :snapAlign="'center'" :breakpoints="breakpoints">
          <Slide v-for="slide in 10" :key="slide" @click="show('m', slide, 10)">
            <div class="carousel__item">
              <!-- <img :src="getAssetsFile(`m${slide}.png`)" class="w-[200px]"> -->
              <img :src="getAssetsFile(`m${slide}.png`)" class="w-[220px] h-[281px] object-cover cursor-pointer">
            </div>
          </Slide>
          <template #addons>
            <Navigation />
          </template>
        </Carousel>
      </div>
    </div>
    <div class="w-screen px-5 gap-3 tablet:grid tablet:grid-cols-2 mb-5 desktop:hidden">
      <img :src="getAssetsFile(`m${pic}.png`)" alt="" v-for="pic in 6" class="w-[100%] h-[100%] object-cover cursor-pointer" @click="show('m', pic, 6)">
    </div>
    <div class="fixed top-0 left-0 w-screen h-screen flex items-center justify-center pic z-[1000]" v-if="showBigPhoto">
      <div class="absolute right-10 top-5 text-xl font-semibold z-20 text-white cursor-pointer" @click="closeBigPhoto()">CLOSE</div>
       <img src="@/assets/img/ca.png" class="rotate-180 w-[50px] absolute top-[50%] left-[10%] translate-y-[-50%] cursor-pointer mobile:left-[2%]" v-if="nowIndex > 1" @click="changePicIndex('back')">
      <img :src="nowPic" class="w-[70vw] h-[80vh] object-contain">
      <img src="@/assets/img/ca.png" class="w-[50px] absolute top-[50%] right-[10%] translate-y-[-50%] cursor-pointer mobile:right-[2%]" v-if="nowIndex < pLimit" @click="changePicIndex('go')">
    </div>
    <div id="SERVICES" class="flex flex-col gap-[50px]">
      <div class="servicePrice">
        <div class="store"><img src="../assets/img/store.png" alt=""></div>
        <div class="service"><img src="../assets/img/service.png" alt=""></div>
      </div>
      <div class="menu"><img src="../assets/img/menu.png" alt=""></div>
      <div class="phoneServicePrice">
        <img src="../assets/img/phoneService1.png" alt="">
        <img src="../assets/img/phoneService2.png" alt="">
        <img src="../assets/img/phoneService3.png" alt="">
        <img src="../assets/img/phoneService4.png" alt="">
        <img src="../assets/img/phoneService5.png" alt="">
        <img src="../assets/img/phoneService6.png" alt="">
      </div>
    </div>
    <!-- Nails Transformed, Beauty Defined. -->
    <div id="GALLERY" class="w-screen flex items-center flex-col py-5 px-20 tablet:px-[28px] tablet:py-[41px]" >
      <span class="text-[#865105] text-[32px] font-bold mb-5 tablet:text-[16px] tablet:text-center" style="font-family: 'InriaSans';letter-spacing: 5px;">Nails Transformed, Beauty Defined.</span>
      <div class="w-full gap-3 mt-5 tablet:grid tablet:grid-cols-2 mb-5 desktop:hidden">
        <img :src="getAssetsFile(`d${pic}.png`)" alt="" v-for="pic in 4" class="w-[100%] h-auto object-cover cursor-pointer" @click="show('d', pic, 4)">
      </div>
      <div class="max-w-[1200px] w-full flex flex-wrap gap-5 justify-between tablet:grid tablet:grid-cols-2">
        <img :src="getAssetsFile(`c${pic}.png`)" alt="" v-for="pic in 10" class="w-[191px] h-[250px] tablet:w-[100%] tablet:h-auto object-cover cursor-pointer" @click="show('c', pic, 10)">
      </div>
      <div class="w-[1280px] flex gap-3 mt-5 px-20 justify-between tablet:hidden">
        <img :src="getAssetsFile(`d${pic}.png`)" alt="" v-for="pic in 4" class="w-[25%] object-cover cursor-pointer" @click="show('d', pic, 4)">
      </div>
    </div>
    <!-- NAIL IT WITH STYLE! -->
    <div v-if="isComputer" id="CONTACT US" class="w-screen flex igArea">
      <img src="@/assets/img/pp.png" alt="" class="w-[30%] object-cover">
      <div class="w-[80%] bg-[#E5D6CD] flex flex-col items-center justify-center gap-5">
        <span class="leading-[38.22px] text-[32px] font-bold mb-5" style="font-family: JosefinSlab;letter-spacing: 5px;">NAIL IT WITH STYLE!</span>
        <div class="flex gap-5 items-center">
          <img src="@/assets/img/ig.svg" alt="" class="w-[58px] h-[58px] mr-5">
          <div class="flex flex-col items-center justify-between">
            <span class="text-[24px]" style="font-family: KaushanScript;letter-spacing: 5px;">Follow & Share</span>
            <a href="https://www.instagram.com/centralnail745/" target="_blank" class="underline text-[24px]" style="font-family: KaiseiDecol;letter-spacing: 2px;">@Centralnail745</a>
          </div>
        </div>
      </div>
    </div>
    <div class="footer">
      <div class="iconList">
        <div v-for="(item, index) in iconList" :key="index">
          <img :src="getAssetsFileIcon(`${index + 1}.svg`)" alt="">
          {{ item }}
        </div>
      </div>
      <div id="CONTACT US" class="CONTACT">
        <div  class="footerSection">
          <div class="footerContent">
            <div class="section1">
              <div class="title">Central Nail Spa 2022</div>
              <div>+1 631-521-6999</div>
              <div>745 Commack Rd, Brentwood, NY <hr>11717, United States</div>
            </div>
            <div class="section2">
              <div class="time">OPENING HOURS</div>
              <div>Monday - Saturday<hr>10:00 - 19:30</div>
              <div>Sunday<hr>10:00 - 18:00</div>
            </div>
            <div class="section3">
              <div v-for="(item, index) in optionList" :key="index" @click="scrollTo(item)">{{ item }}</div>
            </div>
          </div>
          <div class="footerImg"><img src="../assets/img/footerImg.svg" alt=""></div>
        </div>
        <div class="phoneFooterSection">
          <div class="flex gap-5 items-center follow mb-10">
            <img src="@/assets/img/ig.svg" alt="">
            <div class="flex flex-col items-center justify-between">
              <span class="followText">Follow & Share</span>
              <a href="https://www.instagram.com/centralnail745/" target="_blank" class="followLink">@Centralnail745</a>
            </div>
          </div>
          <div class="title">Central Nail Spa 2022</div>
          <div class="phone">+1 631-521-6999</div>
          <div class="address mb-10">745 Commack Rd, Brentwood, NY <hr>11717, United States</div>
          <div class="openHour mb-5">
            <div class="title">OPENING HOURS</div>
            <div class="time">
              <div>Monday - Saturday<hr>10:00 - 19:30</div>
              <div>Sunday<hr>10:00 - 18:00</div>
            </div>
          </div>
          <div class="cursor-pointer my-1" v-for="(item, index) in optionList" :key="index" @click="scrollTo(item)">{{ item }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@media screen and (min-width: 1290px) {
  :deep(.carousel__prev) {
    left: -45px;
  }
  :deep(.carousel__next) {
    right: -45px;
  }
}

.pic {
  background-color: rgba(0,0,0,0.7);
}
.wrapper{
  display: flex;
  flex-direction: column;
  gap: 50px;
}
.header {
  padding: 80px 120px;
  background-color: #E5D6CD;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 35px;
  position: relative;
  &.isPhone{
    padding-top: 80px;
  }
  .paintWall{
    position: absolute;
    bottom: 0;
    background-color: #fff;
    width: 100%;
    height: 180px;
    z-index: 3;
  }
  .section1{
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    &.phoneFixed{
      position: fixed;
      top: 0;
      width: 100%;
      padding: 25px 50px;
      background-color: #E5D6CD;
      z-index: 100;
    }
    .section1Title{
      font-weight: 700;
      font-size: 18px;
    }
    .optionList{
      display: flex;
      gap: 30px;
      >div{
        font-size: 14px;
        font-weight: 400;
        cursor: pointer;
      }
    }
    .menuIcon{
      display: none;
    }
  }
  .section2{
    font-size: 40px;
    font-weight: 400;
    font-family: Jomolhari;
    letter-spacing: 8px;
    text-align: center;
  }
  .section3{
    z-index: 5;
    img{
      width: 620px;
      height: 336px;
    }
  }
}
.servicePrice{
  display: flex;
  align-items: center;
  padding: 0 100px;
  .store{
    width: 50%;
    height: 479px;
    z-index: 3;
    img{
      width: 100%;
      height: 100%;
      object-fit: contain;
    }
  }
  .service{
    width: 50%;
    display: flex;
    justify-content: center;
    background-color: #E5D6CD;
    position: relative;
    img{
      object-fit: contain;
    }
    &::before{
      z-index: 2;
      position: absolute;
      left: -300px;
      top: 0;
      content: '';
      display: block;
      height: 100%;
      width: 300px;
      background-color: #E5D6CD;
    }
  }
}
.menu{
  width: 100%;
  img{
    width: 100%;
  }
}
.phoneServicePrice{
  display: none;
}
.footer{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  // padding: 50px 0 0 ;
  gap: 50px;
  .iconList{
    padding: 0 120px;
    display: flex;
    align-items: center;
    gap: 80px;
    >div{
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      text-align: center;
      font-family: Jomolhari;
      gap: 10px;
      img{
        width: 125px;
        height: 125px;
      }
    }
  }
  .CONTACT{
    width: 100%;
  }
  .footerSection{
    display: flex;
    align-items: center;
    width: 100%;
    height: 341px;
    .footerContent{
      background-color: #E5D6CD;
      width: 60%;
      height: 100%;
      display: flex;
      justify-content: space-evenly;
      align-items: center;
      .section1{
        display: flex;
        flex-direction: column;
        gap: 10px;
        >div{
          font-family: KaiseiDecol;
          font-size: 14px;
          font-weight: 400;
        }
        .title{
          font-weight: 700;
          font-size: 24px;
          font-family: Jomolhari;
        }
      }
      .section2{
        display: flex;
        flex-direction: column;
        gap: 5px;
        >div{
          font-family: 'KaiseiDecol';
          font-size: 14px;
          font-weight: 400;
        }
        .time{
          font-size: 16px;
        }
      }
      .section3{
        >div{
          font-family: KaiseiDecol;
          font-size: 14px;
          font-weight: 400;
          cursor: pointer;
        }
        display: flex;
        flex-direction: column;
        gap: 20px;
      }
    }
    .footerImg{
      width: 40%;
      height: 100%;
      img{
        width: 100%;
        height: 100%;
        object-fit: cover;
      }
    }
  }
  .phoneFooterSection{
    display: none;
  }
}
// @media screen and (min-width: 768px) and (max-width: 1280px) 
@media screen and (max-width: 768px) {
  .header{
    padding: 32px 42px;
    .section1{
      .optionList{
        display: none;
      }
      .menuIcon{
        display: block;
        cursor: pointer;
      }
    }
    .section2{
      font-size: 20px;
    }
    .section3{
      img{
        width: 280px;
        height: 151px;
      }
    }
    .paintWall{
      height: 80px;
    }
  }
  .servicePrice, .menu, .igArea{
    display: none;
  }
  .phoneServicePrice{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    img{
      width: 100%;
    }
  }
  .footer{
    padding: 32px 0;
    background-color: #E5D6CD;
    .iconList, .footerSection{
      display: none;
    }
    .CONTACT{
      .phoneFooterSection{
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 15px;
        .title{
          font-size: 15px;
          font-weight: 700;
          font-family:InriaSans;
        }
        .follow{
          display: flex;
          align-items: flex-end;
          img{
            width: 29px;
            height: 29px;
          }
          .followText{
            font-family: KaushanScript;
            letter-spacing: 5px;
          }
          .followLink{
            text-decoration: underline;
            font-family: KaiseiDecol;
            letter-spacing: 2px;
          }
        }
        .address, .phone{
          text-align: center;
          font-family: KaiseiDecol;
        }
        .openHour{
          display: flex;
          flex-direction: column;
          align-items: center;
          gap: 10px;
          .title{
            font-family: KaiseiDecol;
          }
          .time{
            width: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            div{
              font-size: 14px;
              width: 100%;
              font-family: KaiseiDecol;
            }
          }
        }
      }
    }
  }
}
@media screen and (min-width: 768px) and (max-width: 1024px) {
  .header{
    .section1{
      .optionList{
        display: none;
      }
      .menuIcon{
        display: block;
        cursor: pointer;
      }
    }
  }
  .servicePrice, .menu, .igArea{
    display: none;
  }
  .phoneServicePrice{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    img{
      width: 100%;
    }
  }
  .footer{
    padding: 32px 0;
    background-color: #E5D6CD;
    .iconList, .footerSection{
      display: none;
    }
    .CONTACT{
      .phoneFooterSection{
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 15px;
        .title{
          font-size: 20px;
          font-weight: 700;
          font-family:InriaSans;
        }
        .follow{
          display: flex;
          align-items: flex-end;
          img{
            width: 29px;
            height: 29px;
          }
          .followText{
            font-family: KaushanScript;
            letter-spacing: 5px;
          }
          .followLink{
            text-decoration: underline;
            font-family: KaiseiDecol;
            letter-spacing: 2px;
          }
        }
        .address, .phone{
          text-align: center;
          font-family: KaiseiDecol;
        }
        .openHour{
          display: flex;
          flex-direction: column;
          align-items: center;
          gap: 10px;
          .title{
            font-family: KaiseiDecol;
          }
          .time{
            width: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            div{
              font-size: 14px;
              width: 100%;
              font-family: KaiseiDecol;
            }
          }
        }
      }
    }
  }
}
</style>