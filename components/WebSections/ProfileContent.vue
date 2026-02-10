<template>
  <div class="dashboard">
    <div class="content-section" :class="{ shifted: isSidebarVisible }">
      <button class="toggle-sidebar" @click="toggleSidebar()">
        <img src="/Graphics/NavBars.svg" alt="Menu" />
      </button>
      <transition name="fade" mode="out-in">
        <ProfileDashboard v-if="currentSection == 'dashboard'" />
        <ProfilePreferences v-else-if="currentSection == 'profile'" />
        <ProfileOrders
          v-else-if="currentSection == 'orders'"
          :orders="orders"
        />
        <ProfileWishlist
          v-else-if="currentSection == 'wishlist'"
          @close-sidebar="closeSidebar()"
        />
        <ProfileTicketSupport v-else-if="currentSection == 'support'" />
        <ProfileAdminTickets
          v-else-if="
            currentSection == 'tickets' && userStore.user.role == 'admin'
          "
          @close-sidebar="closeSidebar()"
        />
        <ProfileAdminEditOrders
          v-else-if="
            currentSection == 'edit-orders' && userStore.user.role == 'admin'
          "
          @close-sidebar="closeSidebar()"
        />
        <ProfileAdminEditUsers
          v-else-if="
            currentSection == 'edit-users' && userStore.user.role == 'admin'
          "
          @close-sidebar="closeSidebar()"
        />
        <ProfileAdminEditBlogs
          v-else-if="
            currentSection == 'edit-blogs' && userStore.user.role == 'admin'
          "
        />
      </transition>
    </div>
  </div>
</template>

<script setup>
const userStore = useUserStore();

const props = defineProps({
  isSidebarVisible: Boolean,
  currentSection: String,
});

const { data: orders } = useFetch(`/api/orders/${userStore.user._id}`);

const emit = defineEmits(["toggle-sidebar", "change-section", "close-sidebar"]);

const toggleSidebar = () => {
  emit("toggle-sidebar");
};

const closeSidebar = () => {
  emit("close-sidebar");
};
</script>

<style scoped>
.dashboard {
  display: flex;
  font-family: "Source Sans Pro", Montserrat;
  font-weight: bold;
  transition: transform 0.3s ease;
  width: 100%;
  min-width: 350px;
  position: relative;
  transition: all 0.3s ease;
}

.content-section {
  flex: 1;
  background-color: #737373;
  min-height: 100vh;
  transition: margin-left 0.3s ease;
  width: 100%;
  transition: transform 0.3s ease;
  padding: 0;
  position: relative;
}

.toggle-sidebar {
  background: transparent;
  border: none;
  position: absolute;
  top: 1rem;
  left: 1rem;
  z-index: 900;
  cursor: pointer;
}

.toggle-sidebar img {
  height: 30px;
  width: 30px;
}

@media (max-width: 768px) {
  .content-section {
    margin-left: 0;
  }

  .toggle-sidebar {
    margin: 1rem; /* Ensure there's space from the viewport edges */
  }

  .toggle-sidebar {
    display: block;
    margin-left: 0rem;
    margin-top: 0rem;
  }
}

@media (min-width: 768px) {
  .toggle-sidebar {
    display: none;
  }

}
</style>
