<template>
  <div class="main container">
    <h1>{{ header }}</h1>
    <Breadcrumb :home="home" :model="breadcrumbs" class="p-mb-3" />  
    <Panel class="p-shadow-2 p-mb-3" id="header-contact">
      <template #header>
          <i class="fas fa-paper-plane"></i> <div class="p-panel-title">{{contactPanel.header }}</div>
      </template>
      <div class="p-grid">
        <div class="p-col">
          <div v-html="contactPanel.description"></div>
        </div>
      </div>
    </Panel>
  </div>
  <Footer />
</template>

<script>
export default {
  components: {  },
  setup() {

    const header = "Contact Us"

    const home = { icon: 'pi pi-home', to: { name: 'About'} }
    const breadcrumbs = [
        { label: 'Contact', to: { name: 'Contact'}}
    ]

    const contactPanel = {
      header: " 3DRS contact",
      description: `<p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Dolores fuga veniam inventore, sapiente labore pariatur deserunt! </p>`
    }

    return { header, contactPanel, home, breadcrumbs }
  }
}
</script>

<style>

</style>