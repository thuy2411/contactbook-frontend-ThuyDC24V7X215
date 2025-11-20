<template>
  <div class="page">
    <section class="left">
      <div class="search-wrap">
        <InputSearch v-model="searchText" />
      </div>

      <div class="list-wrap mt-3">
        <h4 class="section-heading">
          Danh bạ
          <i class="fas fa-address-book"></i>
        </h4>

        <ContactList
          v-if="filteredContactsCount > 0"
          :contacts="filteredContacts"
          v-model:activeIndex="activeIndex"
        />
        <p v-else class="muted">Không có liên hệ nào.</p>
      </div>

      <div class="actions mt-3">
        <div class="btn-row">
          <button class="btn btn-primary" @click="refreshList()">
            <i class="fas fa-redo"></i> Làm mới
          </button>
          <button class="btn btn-success" @click="goToAddContact">
            <i class="fas fa-plus"></i> Thêm mới
          </button>
          <button class="btn btn-danger" @click="removeAllContacts">
            <i class="fas fa-trash"></i> Xóa tất cả
          </button>
        </div>
      </div>
    </section>

    <aside class="right">
      <div v-if="activeContact">
        <h4 class="section-heading">
          Chi tiết Liên hệ
          <i class="fas fa-address-card"></i>
        </h4>
        <ContactCard :contact="activeContact" />
      </div>
      <div v-else class="placeholder muted">
        Chọn một liên hệ để xem chi tiết
      </div>
    </aside>
  </div>
</template>
<script>
import ContactCard from "@/components/ContactCard.vue";
import InputSearch from "@/components/InputSearch.vue";
import ContactList from "@/components/ContactList.vue";
import ContactService from "@/services/contact.service";
export default {
  components: {
    ContactCard,
    InputSearch,
    ContactList,
  },
  data() {
    return {
      contacts: [],
      activeIndex: -1,
      searchText: "",
    };
  },
  watch: {
    // Giám sát các thay đổi của biến searchText.
    // Bỏ chọn phần tử đang được chọn trong danh sách.
    searchText() {
      this.activeIndex = -1;
    },
  },
  computed: {
    // Chuyển các đối tượng contact thành chuỗi để tiện cho tìm kiếm.
    contactStrings() {
      return this.contacts.map((contact) => {
        const { name, email, address, phone } = contact;
        return [name, email, address, phone].join("");
      });
    },
    // Trả về các contact có chứa thông tin cần tìm kiếm.
    filteredContacts() {
      if (!this.searchText) return this.contacts;
      return this.contacts.filter((_contact, index) =>
        this.contactStrings[index].includes(this.searchText)
      );
    },
    activeContact() {
      if (this.activeIndex < 0) return null;
      return this.filteredContacts[this.activeIndex];
    },
    filteredContactsCount() {
      return this.filteredContacts.length;
    },
  },
  methods: {
    async retrieveContacts() {
      try {
        this.contacts = await ContactService.getAll();
      } catch (error) {
        console.log(error);
      }
    },
    refreshList() {
      this.retrieveContacts();
      this.activeIndex = -1;
    },
    async removeAllContacts() {
      if (confirm("Bạn muốn xóa tất cả Liên hệ?")) {
        try {
          await ContactService.deleteAll();
          this.refreshList();
        } catch (error) {
          console.log(error);
        }
      }
    },
    goToAddContact() {
      this.$router.push({ name: "contact.add" });
    },
  },
  mounted() {
    this.refreshList();
  },
};
</script>
<style scoped>
.page {
  display: grid;
  grid-template-columns: 1fr 380px;
  gap: 1.5rem;
  align-items: start;
  width: 100%;
}

.left {
  display: flex;
  flex-direction: column;
}
.search-wrap {
  width: 100%;
}
.list-wrap {
  margin-top: 0.6rem;
}
.actions {
  margin-top: 1rem;
}
.btn-row {
  display: flex;
  gap: 0.6rem;
  flex-wrap: wrap;
}
.right {
  min-width: 260px;
}
.placeholder {
  padding: 1rem;
  border-radius: 10px;
  border: 1px dashed var(--color-border);
  background: var(--color-background-soft);
}
.section-heading {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0.6rem;
}
.muted { color: rgba(0,0,0,0.55); }

@media (max-width: 1024px) {
  .page {
    grid-template-columns: 1fr;
  }
  .right {
    order: 3;
  }
}
</style>
