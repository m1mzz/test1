<template>
  <div class="fake-body">
    <div class="p-20-md">
      <div class="container">
        <h1 class="h1">Contacts</h1>
      </div>
      <div>
        <div class="container">
          <div class="row pb">
            <div class="col-6 pr-20-md">
              <Contacts/>
            </div>
            <div class="col-6 pl-20-md">
              <Support/>
            </div>
          </div>
        </div>
      </div>
    </div>
    <Offices/>
    <div class="p-20-md fake-footer background-pattern">
      <ContactUs/>
    </div>
  </div>
</template>

<script>
import Contacts from '~/components/page/contact/Contacts'
import Support from '~/components/page/contact/Support'
import Offices from '~/components/page/contact/Offices'
import ContactUs from '~/components/page/contact/ContactUs'
import contactSeo from '~/assets/js/contactSeo'
export default {
  name: 'ContactPage',
  components: {
    Contacts,
    Support,
    Offices,
    ContactUs
  },
  head() {
    return {
      ...contactSeo
    }
  }
}
</script>

<style>
.fake-body {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}
.fake-footer {
  flex: auto;
}
</style>
