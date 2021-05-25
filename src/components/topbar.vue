<script>
import { authComputed } from '@state/helpers'
import VuePerfectScrollbar from 'vue-perfect-scrollbar'

export default {
  components: {
    VuePerfectScrollbar,
  },
  props: {
    user: {
      type: Object,
      required: false,
      default: () => ({}),
    },
    isMenuOpened: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {}
  },
  computed: {
    ...authComputed,
  },
  methods: {
    toggleMenu() {
      this.$parent.toggleMenu()
    },
    toggleRightSidebar() {
      this.$parent.toggleRightSidebar()
    },
  },
}
</script>

<template>
  <!-- Topbar Start -->
  <div class="navbar navbar-expand flex-column flex-md-row navbar-custom">
    <div class="container-fluid">
      <!-- LOGO -->

      <div class="media user-profile mt-2 mb-2">
        <img
          src="@assets/images/users/avatar-7.jpg"
          class="avatar-sm rounded-circle mr-2"
          alt="Shreyu"
        />
        <img
          src="@assets/images/users/avatar-7.jpg"
          class="avatar-xs rounded-circle mr-2"
          alt="Shreyu"
        />

        <div class="media-body">
          <h6 class="pro-user-name mt-0 mb-0">{{ user.name }}</h6>
          <span class="pro-user-desc">Administrator</span>
        </div>
        <b-dropdown
          variant="black"
          class="align-self-center"
          toggle-class="p-0"
        >
          <template v-slot:button-content>
            <feather type="chevron-down" class="align-middle"></feather>
          </template>

          <b-dropdown-item href="/logout" class="notify-item">
            <feather
              type="log-out"
              class="icon-dual icon-xs mr-2 align-middle"
            ></feather>
            <span>Logout</span>
          </b-dropdown-item>
        </b-dropdown>
      </div>

      <ul
        class="navbar-nav bd-navbar-nav flex-row list-unstyled menu-left mb-0"
      >
        <li class>
          <button
            class="button-menu-mobile open-left"
            :class="{ open: isMenuOpened }"
            @click="toggleMenu"
          >
            <feather type="menu" class="menu-icon align-middle"></feather>
            <feather type="x" class="close-icon"></feather>
          </button>
        </li>
      </ul>

      <ul
        class="navbar-nav flex-row ml-auto d-flex list-unstyled topnav-menu float-right mb-0"
      >
	  
        <b-nav-item-dropdown
          id="bell-notification"
          right
          variant="white"
          class="notification-list"
          title="8 new unread notifications"
          menu-class="dropdown-lg"
        >

          <template v-slot:button-content>
            <feather type="bell" class="align-middle"></feather>
            <span class="noti-icon-badge"></span>
          </template>
          
          <b-tooltip target="bell-notification" placement="left"
            >8 new unread notifications</b-tooltip
          >
          <!-- item-->
          <b-dropdown-text class="noti-title border-bottom pb-2" tag="div">
            <h5 class="m-0 font-size-16">
              <span class="float-right">
                <a href class="text-dark">
                  <small>Clear All</small>
                </a> </span
              >Notification
            </h5>
          </b-dropdown-text>

          <VuePerfectScrollbar v-once class="noti-scroll">
            <!-- item-->
            <b-dropdown-text
              href="javascript:void(0);"
              class="notify-item border-bottom"
            >
              <div class="notify-icon bg-primary">
                <i class="uil uil-user-plus"></i>
              </div>
              <p class="notify-details">
                New user registered.
                <small class="text-muted">5 hours ago</small>
              </p>
            </b-dropdown-text>

            <!-- item-->
            <b-dropdown-text
              href="javascript:void(0);"
              class="notify-item border-bottom"
            >
              <div class="notify-icon">
                <img
                  src="@assets/images/users/avatar-1.jpg"
                  class="img-fluid rounded-circle"
                  alt
                />
              </div>
              <p class="notify-details">Karen Robinson</p>
              <p class="text-muted mb-0 user-msg">
                <small>Wow ! this admin looks good and awesome design</small>
              </p>
            </b-dropdown-text>

            <!-- item-->
            <b-dropdown-text
              href="javascript:void(0);"
              class="notify-item border-bottom"
            >
              <div class="notify-icon">
                <img
                  src="@assets/images/users/avatar-2.jpg"
                  class="img-fluid rounded-circle"
                  alt
                />
              </div>
              <p class="notify-details">Cristina Pride</p>
              <p class="text-muted mb-0 user-msg">
                <small>Hi, How are you? What about our next meeting</small>
              </p>
            </b-dropdown-text>

            <!-- item-->
            <b-dropdown-text
              href="javascript:void(0);"
              class="notify-item border-bottom active"
            >
              <div class="notify-icon bg-success">
                <i class="uil uil-comment-message"></i>
              </div>
              <p class="notify-details">
                Jaclyn Brunswick commented on Dashboard
                <small class="text-muted"> 1 min ago </small>
              </p>
            </b-dropdown-text>

            <!-- item-->
            <b-dropdown-text
              href="javascript:void(0);"
              class="notify-item border-bottom"
            >
              <div class="notify-icon bg-danger">
                <i class="uil uil-comment-message"></i>
              </div>
              <p class="notify-details">
                Caleb Flakelar commented on Admin
                <small class="text-muted"> 4 days ago </small>
              </p>
            </b-dropdown-text>

            <!-- item-->
            <b-dropdown-text href="javascript:void(0);" class="notify-item">
              <div class="notify-icon bg-primary">
                <i class="uil uil-heart"></i>
              </div>
              <p class="notify-details">
                Carlos Crouch liked
                <b>Admin</b>
                <small class="text-muted">13 days ago</small>
              </p>
            </b-dropdown-text>
          </VuePerfectScrollbar>
          <!-- All-->
          <b-dropdown-text
            href="javascript:void(0);"
            class="text-center text-primary notify-item notify-all border-top"
          >
            View all
            <i class="fi-arrow-right"></i>
          </b-dropdown-text>
        </b-nav-item-dropdown>

        <b-nav-item-dropdown
          right
          class="notification-list align-self-center profile-dropdown"
          toggle-class="nav-user mr-0"
        >
          <b-dropdown-item href="/logout" class="notify-item p-0">
            <feather type="log-out" class="icon-dual icon-xs mr-2"></feather>
            <span>Logout</span>
          </b-dropdown-item>
        </b-nav-item-dropdown>
      </ul>
    </div>
  </div>
  <!-- end Topbar -->
</template>

<style lang="scss">
.noti-scroll {
  height: 220px;
}
.ps > .ps__scrollbar-y-rail {
  width: 8px !important;
  background-color: transparent !important;
}
.ps > .ps__scrollbar-y-rail > .ps__scrollbar-y,
.ps.ps--in-scrolling.ps--y > .ps__scrollbar-y-rail > .ps__scrollbar-y,
.ps > .ps__scrollbar-y-rail:active > .ps__scrollbar-y,
.ps > .ps__scrollbar-y-rail:hover > .ps__scrollbar-y {
  width: 6px !important;
}
.button-menu-mobile {
  outline: none !important;
}
</style>
