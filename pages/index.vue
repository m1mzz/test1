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
      title: 'Contact',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'My custom description'
        },
        { hid: 'og:title', name: 'og:title', content: 'About Our Company' },
        { hid: 'og:type', name: 'og:type', content: 'article' },
        {
          hid: 'og:url',
          name: 'og:url',
          content: 'http://www.mysite.com/article/'
        },
        {
          hid: 'og:image',
          name: 'og:image',
          content: 'http://www.mysite.com/articleimage.jpg'
        },
        {
          hid: 'og:site_name',
          name: 'og:site_name',
          content: 'My Company Name'
        },
        { hid: 'fb:app_id', name: 'fb:app_id', content: '1234567890987654321' },
        {
          hid: 'og:description',
          name: 'og:description',
          content: 'A description of our services and company profile.'
        }
      ]
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
