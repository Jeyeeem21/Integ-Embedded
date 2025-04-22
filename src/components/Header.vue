<template>
  <header class="header">
    <div class="header-container">
      <!-- Logo/Brand -->
      <div class="logo">
        <h1>FlowSmart</h1>
      </div>
      
      <!-- Mobile Menu Toggle -->
      <button class="menu-toggle" @click="toggleMenu" aria-label="Toggle menu">
        <span class="bar"></span>
        <span class="bar"></span>
        <span class="bar"></span>
      </button>
      
      <!-- Navigation -->
      <nav class="nav" :class="{ 'nav-open': menuOpen }">
        <ul class="nav-list">
          <li class="nav-item"><a href="#" class="nav-link active">Home</a></li>
          <li class="nav-item"><a href="#" class="nav-link">Analytics</a></li>
          <li class="nav-item"><a href="#" class="nav-link">Reports</a></li>
        </ul>
        
        <!-- User Profile -->
        <div class="user-profile">
          <span class="user-name">John Mark Pagaran</span>
          <div class="user-avatar">JMP</div>
        </div>
      </nav>
    </div>
  </header>
</template>

<script>
export default {
  name: 'Header',
  data() {
    return {
      menuOpen: false
    }
  },
  methods: {
    toggleMenu() {
      this.menuOpen = !this.menuOpen;
      
      // Prevent scrolling when menu is open on mobile
      if (this.menuOpen) {
        document.body.style.overflow = 'hidden';
      } else {
        document.body.style.overflow = '';
      }
    },
    closeMenu() {
      if (this.menuOpen) {
        this.menuOpen = false;
        document.body.style.overflow = '';
      }
    }
  },
  mounted() {
    // Close menu when clicking outside
    document.addEventListener('click', (e) => {
      const header = document.querySelector('.header');
      const menuToggle = document.querySelector('.menu-toggle');
      
      if (this.menuOpen && !header.contains(e.target) || e.target === menuToggle) {
        return;
      }
      
      if (this.menuOpen && !menuToggle.contains(e.target)) {
        this.closeMenu();
      }
    });
    
    // Close menu on window resize (e.g., when switching to desktop)
    window.addEventListener('resize', () => {
      if (window.innerWidth >= 1024 && this.menuOpen) {
        this.closeMenu();
      }
    });
  },
  beforeUnmount() {
    // Clean up event listeners
    document.removeEventListener('click', this.closeMenu);
    window.removeEventListener('resize', this.closeMenu);
  }
}
</script>

<style scoped>
.header {
  background-color: white;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  position: sticky;
  top: 0;
  z-index: 100;
  width: 100%;
}

.header-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 20px;
  height: 70px;
  max-width: 1400px;
  margin: 0 auto;
}

.logo h1 {
  color: #2c3e50;
  font-size: 1.5rem;
  margin: 0;
  font-weight: bold;
}

/* Mobile Menu Toggle */
.menu-toggle {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  width: 30px;
  height: 21px;
  background: transparent;
  border: none;
  cursor: pointer;
  padding: 0;
  z-index: 110;
}

.bar {
  width: 100%;
  height: 3px;
  background-color: #2c3e50;
  border-radius: 3px;
  transition: all 0.3s ease;
}

/* Transform hamburger to X when menu is open */
.nav-open + .menu-toggle .bar:nth-child(1) {
  transform: translateY(9px) rotate(45deg);
}

.nav-open + .menu-toggle .bar:nth-child(2) {
  opacity: 0;
}

.nav-open + .menu-toggle .bar:nth-child(3) {
  transform: translateY(-9px) rotate(-45deg);
}

/* Navigation */
.nav {
  position: fixed;
  top: 70px;
  right: -100%;
  width: 80%;
  max-width: 300px;
  height: calc(100vh - 70px);
  background-color: white;
  box-shadow: -2px 0 10px rgba(0,0,0,0.1);
  transition: right 0.3s ease;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 20px 0;
  overflow-y: auto;
}

.nav-open {
  right: 0;
}

.nav-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.nav-item {
  margin-bottom: 5px;
}

.nav-link {
  display: block;
  padding: 15px 20px;
  color: #2c3e50;
  text-decoration: none;
  transition: background-color 0.2s ease;
  font-weight: 500;
}

.nav-link:hover, .nav-link.active {
  background-color: #f5f5f5;
  color: #4caf50;
}

.nav-link.active {
  border-left: 3px solid #4caf50;
}

/* User Profile */
.user-profile {
  display: flex;
  align-items: center;
  padding: 15px 20px;
  border-top: 1px solid #eee;
  margin-top: auto;
}

.user-name {
  color: #2c3e50;
  font-weight: 500;
  margin-right: 10px;
}

.user-avatar {
  width: 36px;
  height: 36px;
  background-color: #4caf50;
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
}

/* Overlay for mobile */
.header::before {
  content: '';
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0,0,0,0.5);
  z-index: 90;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.3s ease, visibility 0.3s ease;
}

.nav-open ~ .header::before {
  opacity: 1;
  visibility: visible;
}

/* Desktop Styles */
@media (min-width: 1024px) {
  .menu-toggle {
    display: none;
  }
  
  .header-container {
    padding: 0 30px;
  }
  
  .logo h1 {
    font-size: 1.8rem;
  }
  
  .nav {
    position: static;
    width: auto;
    height: auto;
    max-width: none;
    box-shadow: none;
    background-color: transparent;
    padding: 0;
    flex-direction: row;
    align-items: center;
    overflow-y: visible;
  }
  
  .nav-list {
    display: flex;
    margin-right: 20px;
  }
  
  .nav-item {
    margin-bottom: 0;
    margin-right: 5px;
  }
  
  .nav-link {
    padding: 10px 15px;
    border-radius: 4px;
  }
  
  .nav-link.active {
    border-left: none;
    background-color: rgba(76, 175, 80, 0.1);
  }
  
  .user-profile {
    padding: 0;
    border-top: none;
    margin-top: 0;
  }
}
</style>