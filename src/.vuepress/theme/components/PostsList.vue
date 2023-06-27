<template>
  <div class="list-view">
    <div
        v-if="filteredList.length === 0"
        class="empty-list"
    >
     
    </div>
    <ol
        v-else
        class="list"
    >
      <li
          v-for="page of filteredList"
          :key="page.key"
          class="list-item"
      >
        <router-link
            :to="page.path"
            class="item-title"
        >
          {{ page.title }}
          <div>
            {{ page.frontmatter.description }}
          </div>
        </router-link>
        <br>
        <time-ago
            :last-updated="page.frontmatter.date || page.lastUpdated"
            class="item-date"
        />
      </li>
    </ol>
  </div>
</template>

<script>
import TimeAgo from './TimeAgo';

export default {
  components: {
    TimeAgo
  },
  computed: {
    filteredList() {
      console.log(this.$site.pages)
      // Order by publish date, desc
      return this.$site.pages
          .filter(item => item.path.match(/posts\/.*\.html/))
          .sort((a, b) => {
            return new Date(b.frontmatter.date || b.lastUpdated) - new Date(a.frontmatter.date || a.lastUpdated)
          })
    }
  },
}
</script>
