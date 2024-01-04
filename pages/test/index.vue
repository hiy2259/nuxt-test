<template>
  <div>
    <h1>Options API</h1>
    <div>{{ options }}</div>
    <h1>Composition API</h1>
    <div>{{ composition }}</div>
    <div>{{ obj.join(', ') }}</div>
    <div>{{ obj2.items.join(', ') }}</div>
    <div>{{ state1.count1 }}</div>
    <div>{{ state2.count2 }}</div>
    <div>{{ state3.count }}</div>
  </div>
</template>
  
<script lang="ts">
import { onBeforeMount, onMounted, onBeforeUnmount, onUnmounted, computed, ref, reactive } from 'vue'

export default {
  components: {},
  data: () => {
    return {
      options: 'options'
    }
  },
  setup() {
    console.log('=== setup ===');
    const composition = ref<string>('composition');
    const obj = ref(['test1, test2']);
    const obj2 = reactive({ items: ['test4', 'test5'] });
    let state1 = ref( { count1: 0 });
    let state2 = reactive( { count2: 0 });
    let count = ref(0);
    let state3 = reactive({ count });

    let { count1 } = state1.value;
    let { count2 } = state2
    

    count1++;
    count2++;
    state3.count++;

    console.log(state1.value.count1);
    console.log(state2.count2);
    console.log(state3);

    composition.value = 'change setup composition';
    console.log('setup composition = ', composition.value);

    // const transString = computed<string>(() => obj.value.join(''));

    onBeforeMount(() => {
      console.log('=== onBeforeMount ===');
      obj.value = [...obj.value, 'test3'];
      obj2.items.push('test6');
      console.log('컴포넌트가 마운트되기 직전에 호출될 훅을 등록합니다.');
    })
    onMounted(() => {
      console.log('=== onMounted ===');
      console.log('컴포넌트가 마운트된 후 호출될 콜백을 등록합니다.');
    });
    onBeforeUnmount(() => {
      console.log('=== onBeforeUnmount ===');
      console.log('컴포넌트 인스턴스가 마운트 해제되기 직전에 호출될 콜백을 등록합니다.');
    });
    onUnmounted(() => {
      console.log('=== onUnmounted ===');
      console.log('컴포넌트가 마운트 해제된 후 호출될 콜백을 등록합니다.');
    });

    return {
      composition,
      // transString,
      obj,
      obj2,
      state1,
      state2,
      state3
    }
  },
  beforeCreate() {
    // this.options = 'change beforeCreate options';
    console.log('=== beforeCreate ===');
    console.log('인스턴스가 초기화된 후 호출됩니다.');
    // console.log('beforeCreate options = ', this.options);
    
  },
  created() {
    this.options = 'change created options';
    console.log('=== created ===');
    console.log('인스턴스가 모든 상태 관련 옵션 처리를 완료한 후 호출됩니다.')
    console.log('created options = ', this.options);
    
  },
  beforeMount() {
    console.log('=== beforeMount ===');
    console.log('컴포넌트가 마운트되기 직전 호출됩니다.');
  },
  mounted() {
    console.log('=== mounted ===');
    console.log('컴포넌트가 마운트된 후 호출됩니다.');
  },
  beforeDestroy() {
    console.log('=== beforeDestroy ===');
  },
  destroyed() {
    console.log('=== destroyed ===');
  }
}
</script>
  