<script>
export default {
  props: {
    contact: { type: Object, required: true },
  },
};
</script>
<template>
  <div class="contact-card">
    <div class="field"><div class="label">Tên:</div><div class="value">{{ contact.name }}</div></div>
    <div class="field"><div class="label">E-mail:</div><div class="value">{{ contact.email }}</div></div>
    <div class="field"><div class="label">Địa chỉ:</div><div class="value">{{ contact.address }}</div></div>
    <div class="field"><div class="label">Điện thoại:</div><div class="value">{{ contact.phone }}</div></div>
    <div class="field"><div class="label">Liên hệ yêu thích:</div><div class="value"> <i v-if="contact.favorite" class="fas fa-check"></i><i v-else class="fas fa-times"></i></div></div>
  </div>
</template>
