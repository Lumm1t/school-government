<template>
  <div>
    <nuxt-link :to="`./`" class="go-back">Powrót</nuxt-link>

    <div v-if="error">{{ error }}</div>
    <div v-else-if="response.post" class="post">
      <div class="post__header">
        <h1>{{ response.post.title }}</h1>

        <p>Dodano {{ response.post._createdAt | formatDate() }}</p>

        <p v-if="response.post._createdAt !== response.post._updatedAt">
          Zaktualizowano {{ response.post._updatedAt | formatDate() }}
        </p>
      </div>

      <div class="post__content" v-html="response.post.content"></div>
    </div>
  </div>
</template>

<script>
import moment from 'moment';
import { request } from '~/datocms';

export default {
  filters: {
    formatDate(value) {
      return moment(String(value)).locale('pl').format('LLLL');
    },
  },
  data: () => ({
    title: 'Projekty - %s',
    response: '',
    error: null,
  }),
  head() {
    return {
      titleTemplate: this.title,
    };
  },
  async mounted() {
    try {
      this.response = await request({
        query: `
          query Post {
            post(filter: { slug: { eq: "${this.$route.params.slug}" } }) {
              title
              content
              _createdAt,
              _updatedAt
            }
          }
        `,
      });
      this.title = `${this.response.post.title} - Projekty - %s`;
    } catch (e) {
      this.error = e;
    }
  },
};
</script>

<style lang="scss">
@import './assets/sass/variables';

.post {
  margin: 40px auto;
  width: 65vw;
  background-color: var(--backgroundColor-primary);
  border-radius: 5px;
  overflow: hidden;
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  text-align: justify;

  @media (max-width: 1200px) {
    width: 95vw;
  }

  &__header {
    background: linear-gradient(270deg, $mainColor, $secondColor);
    color: white;
    width: 100%;
    text-align: center;
    padding: 10px;
  }

  &__content {
    padding: 20px;
  }

  img {
    width: 100%;
    max-width: 600px;
    height: auto;
  }

  p {
    font-size: 18px;
    margin: 1em 0;
  }

  ol,
  ul {
    margin: 1em 0;
    padding-left: 40px;
  }
}

.go-back {
  z-index: 1;
  top: 5px;
  left: 5px;
  position: absolute;
  font-size: 20px;
  background: linear-gradient(currentColor, currentColor) bottom / 0 0.1em
    no-repeat;
  background-position: left bottom;
  transition: 1s background-size;

  &:hover {
    background-size: 100% 0.1em;
  }
}
</style>
