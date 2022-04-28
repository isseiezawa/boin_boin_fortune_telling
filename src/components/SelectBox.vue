<template>
  <div class="form-group col-6 mx-auto text-center my-3">
    <label for="boin-quantity">ピックアップ数選択</label>
    <select :value="value" @input="$emit('input', $event.target.value)" class="form-control text-center" id="boin-quantity">
      <option v-for="(option, index) in options" :key="index" :value="option.value">{{option.text}}</option>
    </select>
  </div>
</template>

<script>
export default {
  props: {
    value: { type: String, required: true },
    options: { type: Array, required: true }
  }
}
</script>