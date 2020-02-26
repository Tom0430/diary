<template>
  <div>
    <nav class="navbar" role="navigation" aria-label="main navigation">
      <div class="navbar-brand">
        <a class="navbar-item" href="https://bulma.io">
          <img src="https://bulma.io/images/bulma-logo.png" width="112" height="28" />
        </a>
        <a
          role="button"
          class="navbar-burger burger"
          aria-label="menu"
          aria-expanded="false"
          data-target="navbarBasicExample"
        >
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>
      <div id="navbarBasicExample" class="navbar-menu">
        <div class="navbar-start">
          <a class="navbar-item">Home</a>
          <a class="navbar-item">Documentation</a>
          <div class="navbar-item has-dropdown is-hoverable">
            <a class="navbar-link">More</a>
            <div class="navbar-dropdown">
              <a class="navbar-item">About</a>
              <a class="navbar-item">Jobs</a>
              <a class="navbar-item">Contact</a>
              <hr class="navbar-divider" />
              <a class="navbar-item">Report an issue</a>
            </div>
          </div>
        </div>
        <div class="navbar-end">
          <div class="navbar-item">
            <div class="buttons">
              <a class="button is-primary">
                <strong>Sign up</strong>
              </a>
              <a class="button is-light">Log in</a>
            </div>
          </div>
        </div>
      </div>
    </nav>
    <div class="container">
      <div class="tile is-ancestor">
        <div class="tile is-parent" v-for="(post,i) in posts" :key="i">
          <article class="tile is-child box" @click="handleClick(post)">
            <div>
              <p class="title">{{ post.fields.title }}</p>
              <div class="mt-3">
                <img :src="post.fields.heroImage.fields.file.url" />
              </div>
              <!-- <p>{{post.fields.heroImage}}</p> -->
            </div>
            {{post.fields.publishDate}}
            <p class="subtitle">{{post.fields.author.fields.name}}</p>
          </article>
        </div>
      </div>
    </div>
    <footer class="footer">
      <div class="content has-text-centered">
        <p>
          <strong>Bulma</strong> by
          <a href="https://jgthms.com">Jeremy Thomas</a>. The source code is licensed
          <a href="http://opensource.org/licenses/mit-license.php">MIT</a>. The website content
          is licensed
          <a
            href="http://creativecommons.org/licenses/by-nc-sa/4.0/"
          >CC BY NC SA 4.0</a>.
        </p>
      </div>
    </footer>
  </div>
</template>
<script>
import { createClient } from "~/plugins/contentful.js";
const client = createClient();
export default {
  // `env` is available in the context object
  asyncData({ env }) {
    return Promise.all([
      // fetch the owner of the blog
      client.getEntries({
        "sys.id": env.CTF_PERSON_ID
      }),
      // fetch all blog posts sorted by creation date
      client.getEntries({
        content_type: env.CTF_BLOG_POST_TYPE_ID,
        order: "-sys.createdAt"
      })
    ])
      .then(([entries, posts]) => {
        console.log(posts.items[0]);
        // return data that should be available
        // in the template
        return {
          person: entries.items[0],
          posts: posts.items.filter(item => item.fields.heroImage)
        };
      })
      .catch(console.error);
  },
  methods: {
    handleClick(post) {
      console.log(post);
      this.$router.push({
        name: "blogs-slug",
        params: {
          slug: post.fields.title,
          post
        }
      });
    }
  }
};
</script>
<style scoped>
.mt-3 {
  margin-top: 30px;
}
.mt-5 {
  margin-top: 50px;
}
</style>