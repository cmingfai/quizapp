<script setup>
   import q from "../data/quizes.json"
   import {ref, watch} from "vue"

   import Card from "../components/Card.vue"

   import gsap from "gsap"

   const quizes=ref(q)
   const search=ref("")

   const beforeEnter=(el)=> {
    console.log("before enter")
    el.style.opacity=0
    el.style.transform="translateY(-60px)"
   }

   const afterEnter=()=> {
    console.log("after enter")
   }

   const enter=(el)=> {
    // card-enter-to
    console.log("enter")
    gsap.to(
      el,{
        y:0,
        opacity:1,
        duration: 0.4,
        delay: el.dataset.index*0.3
      }
    )
   }

   watch(search,()=> {
    quizes.value=q.filter(quiz => quiz.name.toLowerCase().includes(search.value.toLowerCase()))
   })
</script>

<template>
  <div>
     <header>
      <h1>Quizes</h1>
      <input v-model.trim="search" type="text" placeholder="Search...">
     </header>

     <div class="options-container">
      <TransitionGroup appear
      @beforeEnter="beforeEnter"
      @afterEnter="afterEnter"
      @enter="enter">
        <Card v-for="(quiz,index) in quizes" 
        :key="quiz.id" 
        :quiz="quiz"
        :data-index="index"/>
      </TransitionGroup>
     </div>

  </div>
</template>

<style scoped>

header {
  margin-bottom: 10px;
  margin-top: 30px;
  display: flex;
  align-items: center;
}

header h1 {
  font-weight: bold;
  margin-right: 30px;
}

header input {
  border: none;
  background-color: rgba(128,128,128,0.1);
  padding: 10px;
  border-radius: 5px;
}

.options-container {
  display: flex;
  flex-wrap: wrap;
  margin-top: 40px;
}

/* CARD */
/* .card-enter-from {
  opacity: 0;
  transform: translateY(-50px)
}
.card-enter-to {
  opacity: 1;
  transform: translateY(0)
}
.card-enter-active {
  transition: all 0.4s ease
} */

</style>