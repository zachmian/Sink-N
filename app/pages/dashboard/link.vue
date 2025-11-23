<script setup>
const route = useRoute()

const link = ref({})
const id = computed(() => link.value.id)

provide('id', id)

const slug = computed(() => route.query.slug)

async function getLink() {
  if (!slug.value)
    return

  try {
    const data = await useAPI('/api/link/query', {
      query: {
        slug: slug.value,
      },
    })
    // data.id = 'y1c4fhirl5'
    link.value = data
  }
  catch (error) {
    link.value = {}
  }
}

function updateLink(link, type) {
  if (type === 'delete') {
    navigateTo('/dashboard/links', {
      replace: true,
    })
  }
}

watch(slug, () => {
  getLink()
}, { immediate: true })
</script>

<template>
  <main class="space-y-6">
    <DashboardBreadcrumb title="Link" />
    <DashboardLinksLink
      v-if="link.id"
      :link="link"
      @update:link="updateLink"
    />
    <DashboardAnalysis
      v-if="link.id"
      :link="link"
    />
  </main>
</template>
