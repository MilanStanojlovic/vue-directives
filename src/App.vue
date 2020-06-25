<template>
  <div class="container">
    <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
      <h1>Built-in Directives</h1>
      <p v-text="'Some text'"></p>
      <p v-html="'<strong>Some strong text</strong>'"></p>
    </div>
    <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
      <h1>Custom Directives</h1>
      <!-- <p v-highlight>Color</p> -->
      <!-- <p v-highlight="'red'">Color</p> -->
      <p v-highlight:background.delayed="'red'">Color</p>
      <p
        v-localHighlight:background.delayed.blink="{mainColor: 'red', secondColor:'green', delay: 500}"
      >Local highlight</p>
    </div>
  </div>
</template>

<script>
export default {
  directives: {
    localHighlight: {
      bind(el, binding, vnode) {
        let delay = 0;
        if (binding.modifiers["delayed"]) {
          delay = 3000;
        }
        if (binding.modifiers["blink"]) {
          let mainColor = binding.value.mainColor;
          let secondColor = binding.value.secondColor;
          let current = mainColor;

          setTimeout(() => {
            setInterval(() => {
              current == secondColor
                ? (current = mainColor)
                : (current = secondColor);
              if (binding.arg == "background") {
                el.style.backgroundColor = current;
              } else {
                el.style.color = current;
              }
            }, binding.value.delay);
          }, delay);
        } else {
          setTimeout(() => {
            if (binding.arg == "background") {
              el.style.backgroundColor = binding.value.mainColor;
            } else {
              el.style.color = binding.value.mainColor;
            }
          }, delay);
        }
      }
    }
  }
};
</script>

<style>
</style>