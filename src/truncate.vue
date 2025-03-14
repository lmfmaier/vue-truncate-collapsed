<template>
  <div>
    <div v-if="!show && type !== 'html'">
      <span :class="textClass">
        {{ truncate(text) }}
      </span>
      <a
        v-if="text.length >= length"
        :class="actionClass"
        @click.stop="toggle()">{{ clamp }}</a>
    </div>
    <div v-else-if="!show && type === 'html'">
      <span
        :class="textClass"
        v-html="truncate(text)" />
      <a
        v-if="text.length >= length"
        :class="actionClass"
        @click.stop="toggle()">{{ clamp }}</a>
    </div>
    <div v-if="show && type !== 'html'">
      <span>{{ text }}</span>
      <a
        v-if="text.length >= length"
        :class="actionClass"
        @click.stop="toggle()">{{ less }}</a>
    </div>
    <div v-else-if="show && type === 'html'">
      <div
        v-if="text.length >= length"
        v-html="text" />
      <a
        v-if="text.length >= length"
        :class="actionClass"
        @click.stop="toggle()">{{ less }}</a>
      <p v-else>
        {{ h2p(text) }}
      </p>
    </div>
  </div>
</template>

<script>
const h2p = require('html-truncate');

export default {
  name: 'Truncate',

  props: {
    collapsedTextClass: {
      type: String,
      default: '',
    },
    text: {
      type: String,
      required: true,
    },
    clamp: {
      type: String,
      default: 'Read More',
    },
    length: {
      type: Number,
      default: 100,
    },
    less: {
      type: String,
      default: 'Show Less',
    },
    type: {
      type: String,
      default: 'text',
    },
    actionClass: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      show: false,
      counter: this.length,
    };
  },
  computed: {
    textClass() {
      return (this.text.length > this.length && this.collapsedTextClass) ? this.collapsedTextClass : '';
    },
  },
  methods: {
    truncate(string) {
      if (string) {
        if (this.type === 'html') return h2p(string, this.length);

        return string.toString().substring(0, this.length);
      }

      return '';
    },
    toggle() {
      const toggled = !this.show;

      this.show = toggled;
      this.$emit('toggle', toggled);
    },

    h2p(text) {
      return h2p(text);
    },
  },
};
</script>

<style lang="css" scoped>
  a {
    cursor: pointer;
  }
</style>
