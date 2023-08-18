<template>
  <div>
    <p>O - {{ o_firstName }}</p>
    <p>c - {{ c_firstName }}</p>
    <p>grett {{ greet }}</p>

    <p>Last NAME {{ c_lastName }}</p>
    {{ userDetailState }}
    <p>r_place {{ place }}</p>
    <button @click="changeName">change optional</button>
    <button @click="changeNameComposition">change composition</button>
    <input type="text" name="" id="" v-model="c_firstName" />
    <input type="text" name="" id="" v-model="c_place" />
    <p>comp- computed : {{ computedRefFullDetails }}</p>
    <input type="text" name="" id="" v-model="o_firstName" />
    <input type="text" name="" id="" v-model="name" />
    <hr />
    <input type="text" name="" id="" v-model="name" />
    <input type="text" name="" id="" v-model="place" />
    <p>comp- computed reactive : {{ computedRefFullDetailsReactive }}</p>
  </div>
</template>

<script>
import { ref, reactive, to, toRefs, computed, watch } from "vue";

export default {
  data() {
    return {
      o_firstName: "Options api",
    };
  },
  setup() {
    //    const c_firstName="Compostion api"
    const c_firstName = ref("compostion api");
    const c_place = ref("c_place");
    const userDetailState = reactive({
      name: "sample",
      age: 29,
      place: "calicut",
    });
    let c_lastName = "last name";

    const greet = `hello greet ${c_firstName.value}`;
    setTimeout(() => {
      c_firstName.value = "changed first";
      c_lastName = "change last";
      userDetailState.place = "usa";
    }, 2000);
    function changeNameComposition() {
      c_firstName.value = "Change button clicked";
      userDetailState.age = "age changed";
    }
    let computedRefFullDetails = computed(
      () => c_firstName.value + c_place.value
    );
    watch(()=>{
        return{...userDetailState}}, (newValue, oldValue) => { // if we make in 
      console.log("oldValue name", oldValue.name);
      console.log("newValue name", newValue.name);
      console.log("oldValue place", oldValue.place);
      console.log("newValue place", newValue.place);
    });
    // if we need only one data it support multiple values by given to array
    // watch(c_firstName, (newValue, oldValue) => {
    //   console.log("oldValue", oldValue);
    //   console.log("newValue", newValue);
    // });
    // let dataListToWatch=[]
    watch(
      [c_firstName, c_place],
      (newValues, oldValues) => {
        if (oldValues[0] !== newValues[0]) {
          console.log("c_firstName oldValue", oldValues[0]);
          console.log("c_firstName newValue", newValues[0]);
        }
        if (oldValues[1] !== newValues[1]) {
          console.log("c_place oldValue", oldValues[1]);
          console.log("c_place newValue", newValues[1]);
        }
      },
      {
        immediate: true,
      }
    );

    let computedRefFullDetailsReactive = computed(
      () => userDetailState.name + userDetailState.place
    );
    return {
      c_firstName,
      c_place,
      greet,
      c_lastName,
      userDetailState,
      ...toRefs(userDetailState),
      changeNameComposition,
      computedRefFullDetails,
      computedRefFullDetailsReactive,
    };
  },
  methods: {
    changeName() {
      console.log("clicked");
      this.o_firstName = "o_changed";
      this.c_firstName = "c_changed";
      this.c_lastName = "c_last_changed";
      this.userDetailState.place = "india";
    },

  },
};
</script>

<style scoped>
</style>