<template>
  <div>
    <!-- Page Sidebar Start-->
    <div class="page-sidebar">
      <div class="main-header-left d-none d-lg-block">
        <div class="logo-wrapper">
          <router-link to="/dashboard">
          <h1 class="m-4">EVALLEY</h1>
          </router-link>
        </div>
      </div>
      <div class="sidebar custom-scrollbar">
        <div class="sidebar-user text-center">
          <div>
            <img
              class="img-60 rounded-circle lazyloaded blur-up"
              src="../../assets/images/dashboard/man.png"
              alt="#"
            />
          </div>
          <h6 class="mt-3 f-14">JOHN</h6>
          <p>general manager.</p>
        </div>
        <ul
          class="sidebar-menu"
          id="myDIV"
        >
          <li
            v-for="(menuItem, index) in menuItems"
            :key="index"
            :class="{ active: menuItem.active }"
          >
            <!-- Sub -->
            <a
              href="javascript:void(0)"
              class="sidebar-header"
              v-if="menuItem.type == 'sub'"
              @click="setNavActive(menuItem, index)"
            >
             <feather :type="menuItem.icon"> </feather>
              <span>
                {{ menuItem.title }}
              </span>
              <i
                class="fa fa-angle-right pull-right"
                v-if="menuItem.children"
              ></i>
            </a>
            <router-link
              :to="menuItem.path"
              class="sidebar-header"
              v-if="menuItem.type == 'link'"
              router-link-exact-active
            >
              <feather :type="menuItem.icon" class="middle"></feather>
              <span>
                {{ menuItem.title }}
              </span>
              <i
                class="fa fa-angle-right pull-right"
                v-if="menuItem.children"
              ></i>
            </router-link>

            <ul
              class="sidebar-submenu"
              v-if="menuItem.children"
              :class="{ 'menu-open': menuItem.active }"
            >
              <li
                v-for="(childrenItem, index) in menuItem.children"
                :key="index"
                :class="{ active: childrenItem.active }"
              >
                <!-- Sub -->
                <a
                  href="javascript:void(0)"
                  v-if="childrenItem.type == 'sub'"
                  @click="setNavActive(childrenItem, index)"
                >
                  <i class="fa fa-circle"></i>
                  {{ childrenItem.title }}
                  <i
                    class="fa fa-angle-right pull-right"
                    v-if="childrenItem.children"
                  ></i>
                </a>
                <!-- Link -->
                <!--{{ childrenItem.children }}-->
                <!--{{ childrenItem.path }}-->
                <span
                  v-for="(item, index) in childrenItem.children"
                  :key="index"
                >
                </span>

                <router-link
                  :to="childrenItem.path"
                  v-if="childrenItem.type == 'link'"
                  router-link-exact-active
                >
                  <i class="fa fa-circle"></i>
                  {{ childrenItem.title }}
                  <i
                    class="fa fa-angle-down pull-right"
                    v-if="childrenItem.children"
                  ></i>
                </router-link>
              </li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
    <!-- Page Sidebar Ends-->
  </div>
</template>
<script>
import { mapState } from "vuex";
export default {
  name: "Sidebar",
  data() {
    return {
      width: 0,
      height: 0,
    };
  },
  computed: {
    ...mapState({
      menuItems: state => state.menu.data
    })
  },
  created() {
    window.addEventListener("resize", this.handleResize);
    this.handleResize();
  },
  destroyed() {
    window.removeEventListener("resize", this.handleResize);
  },
  mounted() {
    this.menuItems.filter(items => {
      if (items.path === this.$route.path)
        this.$store.dispatch("menu/setActiveRoute", items);
      if (!items.children) return false;
      items.children.filter(subItems => {
        if (subItems.path === this.$route.path)
          this.$store.dispatch("menu/setActiveRoute", subItems);
        if (!subItems.children) return false;
        subItems.children.filter(subSubItems => {
          if (subSubItems.path === this.$route.path)
            this.$store.dispatch("menu/setActiveRoute", subSubItems);
        });
      });
    });
  },
  methods: {
    setNavActive(item) {
      this.$store.dispatch("menu/setNavActive", item);
    },
    handleResize() {
      this.width = window.innerWidth - 310;
    }
  }
};
</script> 
<style scoped>

</style>
