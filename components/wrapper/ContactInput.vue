<template>
  <label class="pos-rel w-100 inp-wrap" >
    <input
      class="contact-input mb-2 w-100"
      :class="{'error-validate': error}"
      :type="type"
      required
      :value="value"
      :name="name.toLowerCase()"
      @input="onInput(name, $event.target.value)"
    >
    <span class="label">{{ name }}</span>
    <span v-if="error" class="icon" :class="{'error-validate': error}">
      <svg
        aria-hidden="true"
        focusable="false"
        role="img"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 512 512"><path fill="currentColor" d="M504 256c0 136.997-111.043 248-248 248S8 392.997 8 256C8 119.083 119.043 8 256 8s248 111.083 248 248zm-248 50c-25.405 0-46 20.595-46 46s20.595 46 46 46 46-20.595 46-46-20.595-46-46-46zm-43.673-165.346l7.418 136c.347 6.364 5.609 11.346 11.982 11.346h48.546c6.373 0 11.635-4.982 11.982-11.346l7.418-136c.375-6.874-5.098-12.654-11.982-12.654h-63.383c-6.884 0-12.356 5.78-11.981 12.654z"></path>
      </svg>
      <small v-if="errorMsg" class="tooltip">{{ errorMsg }}</small>
    </span>
  </label>
</template>
<script>
export default {
  name: 'ContactInput',
  props: {
    name: {
      type: String,
      required: true
    },
    error: {
      type: Boolean,
      default: false
    },
    errorMsg: {
      type: String
    },
    type: {
      type: String,
      default: 'text'
    },
    value: {
      required: true
    }
  },
  methods: {
    onInput(...args) {
      this.$emit('onInput', ...args)
    }
  }
}
</script>
<style lang="scss" scoped>
.icon {
  width: 25px;
  position: absolute;
  top: 50%;
  right: 5px;
  transform: translateY(-50%);

  &:hover .tooltip {
    visibility: visible;
    z-index: 100;
    opacity: 1;
  }
}
.tooltip {
  visibility: hidden;
  position: absolute;
  opacity: 0;
  transition: opacity 0.3s ease-in-out, visibility 0.4s;
  left: 50%;
  bottom: 100%;
  padding: 0 20px;
  transform: translate(-50%, -20%);
  border-radius: 5px;
  background-color: rgba(96, 96, 96, 0.9);
  width: 150px;
  width: max-content;
  color: #fff;

  &:before {
    content: '';
    border-style: solid;
    border-width: 0.75em 0.7em 0;
    border-color: rgba(96, 96, 96, 0.9) transparent transparent transparent;
    top: 100%;
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
  }
}

.inp-wrap {
  font-size: 18px;
  line-height: 50px;
  display: block;
}
.label {
  position: absolute;
  top: 0;
  left: 0;
}
.contact-input {
  font-weight: inherit;
  line-height: inherit;
  padding-left: 70px;
  padding-right: 30px;
  border-width: 0 0 1px;
  border-color: #333;
  color: #333;
  background: 0 0;
  transition: color 0.3s;

  &:focus {
    color: #fff;
    border-color: #fff;
    outline: none;

    ~ * {
      color: inherit;
    }
  }
  ~ * {
    transition: color 0.3s;
    color: #000;
  }
}
</style>
