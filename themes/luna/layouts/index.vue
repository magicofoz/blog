<template>
  <div>
    <Header />
    <div class="main">
      <slot name="body" v-if="page.attributes.compileTemplate" />  
      <div v-else v-html="page.body"></div>
      <div class="cat" v-if="page.attributes.type === 'tag'">
        &lt;{{ page.tag }}/&gt;
      </div>
      <div class="cat" v-if="page.attributes.type === 'category'">
        &lt;{{ page.category }}/&gt;
      </div>
      <div class="post-list">
        <div class="post-item" v-for="post in page.posts">
          <h2 class="post-title">
            <router-link :to="post.permalink">
              {{  post.attributes.title }}
            </router-link>
          </h2>
        </div>
      </div>
      <div class="pagination" v-if="page.pagination">
        <router-link 
          class="prev" 
          title="旧的文章" 
          v-if="page.pagination.hasPrev"
          :to="page.pagination.prevLink">
          旧的文章
        </router-link>
        <router-link 
          class="next" 
          title="新的文章" 
          v-if="page.pagination.hasNext"
          :to="page.pagination.nextLink">
          新的文章
        </router-link>
      </div>
    </div>
    <Footer />
  </div>
</template>

<script>
import Header from '@theme/components/Header'
import Footer from '@theme/components/Footer'

export default {
  props: ['page'],

  head() {
    return {
      title: this.$siteData.title,
      meta: [
        {
          name: 'description',
          content: this.$siteData.description
        }
      ],
      link: [
        this.$feedLink
      ].filter(Boolean)
    }
  },

  components: {
    Header,
    Footer
  }
}
</script>

<style scoped lang="stylus">
.post-title
  font-weight: 300
  font-size: 2rem
  margin: 0 0 15px 0
  a
    color: #b58900
    text-decoration: none
    &:hover
      color: #ff69b4

.pagination
  a
    margin-right: 20px
    color: #b58900
    height: 38px
    line-height: 38px
    transition: all .3s ease
    position: relative
    overflow: hidden
    display: inline-block
    background: #eee8d5
    padding: 0 15px
    text-decoration: none
    &:before
      content: attr(title)
      width: 100%
      left: 0
      transform: translateX(-100%)
      text-align: center
      top: 0
      bottom: 0
      background-color: #b58900
      position: absolute
      transition: transform .3s ease
      z-index: 1
      color: #eee8d5
    &:hover
      color: #eee8d5
      &:before
        transform: translateX(0)

.cat
  font-size: 5rem
  color: #586e75
  margin-bottom: 20px

@media screen and (max-width: 768px)
  .post-title
    font-size: 16px
    
  .cat
    font-size: 2rem
</style>