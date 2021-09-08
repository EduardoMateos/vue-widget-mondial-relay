
<template>
  <div class="mondial-relay-alert mondial-relay-alert--info">
    {{message}}
  </div>
</template>


<script>
export default {
  props: ['message']
};
</script>

<style lang="scss">
.mondial-relay-alert {
  padding: 10px;
  margin: 10px;
  color: #333;
  background: #fafafa;
  border: 1px solid #ccc;

  &--info{
    color: #204a8e;
    background: #c9ddff;
    border: 1px solid #4c699b;
  }
  
}

</style>
