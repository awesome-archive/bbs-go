<template>
  <section class="main">
    <top-notice />
    <div class="container main-container is-white left-main">
      <div class="left-container">
        <topics-nav :nodes="nodes" :current-node-id="0" />
        <topic-list :topics="topicsPage.results" :show-ad="false" />
        <pagination
          :page="topicsPage.page"
          url-prefix="/topics/node/newest?p="
        />
      </div>
      <topic-side />
    </div>
  </section>
</template>

<script>
import TopicSide from '~/components/TopicSide'
import TopicsNav from '~/components/TopicsNav'
import TopicList from '~/components/TopicList'
import Pagination from '~/components/Pagination'
import TopNotice from '~/components/TopNotice'

export default {
  components: {
    TopicSide,
    TopicsNav,
    TopicList,
    Pagination,
    TopNotice
  },
  async asyncData({ $axios, query }) {
    try {
      const [user, nodes, topicsPage] = await Promise.all([
        $axios.get('/api/user/current'),
        $axios.get('/api/topic/nodes'),
        $axios.get('/api/topic/topics', {
          params: {
            page: query.p || 1
          }
        })
      ])
      return { user, nodes, topicsPage }
    } catch (e) {
      console.error(e)
    }
  },
  head() {
    return {
      title: this.$siteTitle('全部话题'),
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.$siteDescription()
        },
        { hid: 'keywords', name: 'keywords', content: this.$siteKeywords() }
      ]
    }
  }
}
</script>

<style lang="scss" scoped></style>
