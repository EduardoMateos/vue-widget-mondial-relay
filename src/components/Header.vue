
<template>
  <div class="mondial-relay-header">
    <img
      src="https://widget.mondialrelay.com/parcelshop-picker/v4_0/css/imgs/mr-64.png"
      alt="mondial relay"
    />
    <div class="mondial-relay-form">
      <div class="mondial-relay-form__label">
        <label for="cp">Código postal</label>
      </div>
      <div class="mondial-relay-form__input">
        <input id="cp" name="cp" v-model="cp" required size="8" />
        <button type="button" v-on:click="search()">Buscar</button>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: "Header",
  data() {
    return {
      cp: 28037,
    };
  },
  methods: {
    search() {
      this.$emit('searchByCp', this.cp)
    },
  },
};
</script>

<style lang="scss">
.mondial-relay-header {
  display: flex;
  flex-wrap: wrap;
  margin: 12px;
}

.mondial-relay-form {
  margin: auto;
  display: flex;

  &__label {
    margin: auto;
    margin-right: 28px;
  }

  &__input {
    display: flex;
    input {
      display: block;
      width: 100%;
      padding: 0.375rem 0.75rem;
      font-size: 1rem;
      line-height: 1.5;
      color: #495057;
      background-color: #fff;
      background-clip: padding-box;
      border: 1px solid #ced4da;
      border-radius: 0.25rem;
      transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
    }
  }
}
</style>
