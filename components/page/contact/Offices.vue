<template>
  <section :class="{'theme-dark': themeDark}" class="container">
    <div class="col-6 big-padding">
      <h2 class="h2">Our Offices</h2>
      <ul class="tab-head list-reset row">
        <li class="d-i-block" v-for="({}, name) in tabs" :key="name">
          <button
            class="btn-reset tab-btn upper p3"
            :class="{'highlight-text': name === activeTab}"
            :disabled="name === activeTab"
            @click="tabClick(name)"
          >{{name}}</button>
        </li>
      </ul>
      <div class="tab-body">
        <div v-show="name === activeTab" v-for="({title, address}, name) in tabs" :key="name">
          <h3 class="h3">{{title}}</h3>
          <p v-html="address"></p>
        </div>
      </div>
    </div>
    <div class="col-6">
      <Map :coords="coords"/>
    </div>
  </section>
</template>
<script>
export default {
  name: 'ContactOffices',
  props: {
    themeDark: {
      default: true,
      type: Boolean
    }
  },
  data() {
    return {
      tabs: {
        Kyiv: {
          coords: [],
          title: 'Global Message Services Ukraine LLC',
          address: 'Kuiv, Stepan Bandera, 33 <br> 02066 <br> Ukraine'
        },
        'New York': {
          coords: [],
          title: '',
          address: ''
        },
        Guangzhou: {
          coords: [],
          title: '',
          address: ''
        },
        Barcelona: {
          coords: [],
          title: '',
          address: ''
        }
      },
      activeTab: 'Kyiv'
    }
  },
  methods: {
    tabClick(key) {
      this.activeTab = key
    }
  },
  computed: {
    coords() {
      return this.tabs[this.activeTab].coords
    }
  }
}
</script>
<style lang="scss" scoped>
.theme-dark {
  background: #262626;
  color: #fff;
}
.big-padding {
  padding-top: 90px;
  padding-bottom: 140px;
}
.tab-btn {
  color: inherit;
  font-size: 11px;
  letter-spacing: 1.5px;
}
</style>
