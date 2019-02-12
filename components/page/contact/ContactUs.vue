<template>
  <section id="contact_form" class="container big-padding text-white">
    <h2 class="h2">Contact Us</h2>
    <div class="row">
      <form
        class="col-6 pr-10-md"
        :action="sendUrl"
        method="post"
        @submit.prevent="onSubmit"
      >
        <div v-for="(input, name) in form" :key="name">
          <ContactInput
            :name="name"
            :value="input.value"
            :type="input.type"
            :error="input.error"
            :errorMsg="input.errorMsg"
            @onInput="onInput"
          />
        </div>
        <input v-model="agree" id="agree" type="checkbox" class="custom-checkbox visually-hidden" name="agree">
        <label for="agree" class="fs-18">I agree the processing of personal data</label>
        <button type="submit" class="send-btn btn-reset bold upper mt-20" :disabled="disabledSubmit && !withoutJs">
          Get in touch<span v-if="sending" class="spin"></span></button>
        <span v-if="errorSend || successSend" class="h4 bold ml-10 d-i-block">
          Send
          {{ successSend ? 'Succes' : '' }}
          {{ errorSend ? 'Error' : '' }}
        </span>
      </form>
      <div class="col-6 pl-10-md">
        <p class="text-black mt-2 fs-18">
          Please tell us more about your request and give us info about your company and country
        </p>
      </div>
    </div>
  </section>
</template>
<script>
import ContactInput from '@/components/wrapper/ContactInput'
import Vue from 'vue'
export default {
  name: 'ContactUs',
  components: {
    ContactInput
  },
  data() {
    return {
      sendUrl: 'https://httpbin.org/post',
      form: {
        Name: {
          value: '',
          errorMsg: 'Enter valid name',
          pattern: /^[а-яА-ЯёЁa-zA-Z0-9]+$/,
          type: 'text'
        },
        Phone: {
          value: '',
          errorMsg: 'Enter valid phone',
          // eslint-disable-next-line
          pattern: /^(?:(?:\(?(?:00|\+)([1-4]\d\d|[1-9]\d?)\)?)?[\-\.\ \\\/]?)?((?:\(?\d{1,}\)?[\-\.\ \\\/]?){0,})(?:[\-\.\ \\\/]?(?:#|ext\.?|extension|x)[\-\.\ \\\/]?(\d+))?$/,
          type: 'tel'
        },
        'E-mail': {
          value: '',
          errorMsg: 'Enter valid e-mail',
          // eslint-disable-next-line
          pattern: /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/,
          type: 'email'
        }
      },
      errorInputs: [],
      agree: false,
      sending: false,
      errorSend: false,
      successSend: false,
      withoutJs: true
    }
  },
  computed: {
    disabledSubmit() {
      if (!this.agree) return true
      for (const key in this.form) {
        const item = this.form[key]
        if (!item.activated || item.error) {
          return true
        }
      }
      return false
    }
  },
  created() {
    for (const key in this.form) {
      const item = this.form[key]
      Vue.set(item, 'error', false)
      Vue.set(item, 'activated', false)
    }
  },
  mounted() {
    if (process.browser) {
      this.withoutJs = false
    }
  },
  methods: {
    onInput(name, value) {
      const pattern = this.form[name].pattern
      const error = !pattern.test(value)
      const index = this.errorInputs.indexOf(name)

      this.form[name].value = value
      if (!this.form[name].activated) {
        this.form[name].activated = true
      }
      if (index === -1 && error) {
        this.errorInputs.push(name)
        this.form[name].error = true
      } else if (!error) {
        this.errorInputs.splice(index, 1)
        this.form[name].error = false
      }
    },
    onSubmit() {
      this.successSend = false
      this.errorSend = false
      this.sending = true
      const sendForm = {}
      for (const key in this.form) {
        const item = this.form[key]
        sendForm[key] = item.value
      }
      this.$axios
        .$post(this.sendUrl, sendForm)
        .then(({ data }) => {
          this.clearForm()
          this.successSend = true
          this.sending = false
        })
        .catch(e => {
          this.sending = false
          this.errorSend = true
        })
    },
    clearForm() {
      for (const key in this.form) {
        const item = this.form[key]
        item.value = ''
        item.activated = false
      }
      this.agree = false
    }
  }
}
</script>
<style lang="scss" scoped>
.send-btn {
  padding: 30px 90px;
  background: #262626;
  font-size: 11px;
  transition: background-color 0.3s;

  &:disabled {
    cursor: not-allowed;
    background: gray;
  }

  &:focus {
    outline: none;
    border-bottom: 3px solid #fff;
  }
}
.big-padding {
  padding-top: 30px;
  padding-bottom: 30px;
}
.custom-checkbox {
  + label {
    padding-left: 30px;
    position: relative;
    display: block;

    &:before {
      position: absolute;
      content: '';
      top: 50%;
      left: 0;
      transform: translateY(-50%);
      width: 15px;
      height: 15px;
      background: #fff;
    }
  }

  &:focus + label {
    color: #000;
    text-decoration: underline;
  }

  &:checked {
    + label {
      &:before {
        background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' version='1' viewBox='0 0 1000 1000'%3E%3Cpath d='M729 231c-3 1-8 4-10 7L566 467 415 695c-2 1-20-20-73-84-39-48-73-88-76-89-9-5-19-5-28-1-15 8-21 26-14 40a3560 3560 0 0 0 180 216c8 4 21 3 29-3 5-3 43-60 233-346l105-159v-10c0-12-3-18-13-26-6-4-22-6-29-2z'/%3E%3C/svg%3E");
      }
    }
  }
}
</style>
