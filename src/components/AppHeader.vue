<template>
  <header class="app-header">
    <nav class="navbar navbar-expand">
      <a href="/" class="navbar-brand">Ứng dụng Quản lý danh bạ</a>
      <div class="mr-auto navbar-nav">
        <li class="nav-item">
          <router-link :to="{ name: 'contactbook' }" class="nav-link">
            Danh bạ
            <i class="fas fa-address-book"></i>
          </router-link>
        </li>
      </div>
    </nav>
  </header>
</template>
