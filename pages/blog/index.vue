<template>
  <div class="relative pb-8 mt-4">
    <div class="container flex flex-col items-center justify-center px-3 bg-transparent md:mx-auto sm:px-0">
      <div class="flex flex-col items-center justify-center">
        <img class="mb-2" src="~/assets/The Blog Plaque.svg" alt="Plaque" />
        <p class="w-2/3 text-sm leading-4 text-center">J’écris des articles quand j’ai le temps, je parle de développement web et de mes expériences.</p>
      </div>
      <hr class="w-32 mx-auto my-10 border-black" />
      <div class="flex flex-col items-stretch justify-start max-w-3xl sm:ml-16">
        <ArticleCard v-for="(blog, index) in articles" :key="index" :index="index" :article-info="blog" class="mb-4" />
      </div>
    </div>
    <img class="z-n1 leaf1" src="~/assets/images/leaf1.png" alt="" />
    <img class="z-n1 leaf2" src="~/assets/images/leaf2.png" alt="" />
  </div>
</template>


<style lang="scss" scoped>
.leaf1 {
  position: absolute;
  top: 0%;
}
.leaf2 {
  position: absolute;
  right: 0;
  bottom: -3rem;
}

@media screen and (max-width: 1024px) {
  .leaf1,
  .leaf2 {
    filter: opacity(0.2);
  }
}
</style>


<script>
import ArticleCard from "~/components/ArticleCard";

export default {
  components: {
    ArticleCard,
  },

  async asyncData({ $content, params }) {
    const articles = await $content("blog", params.slug).only(["title", "description", "landingImg", "slug", "author"]).sortBy("ctime", "desc").fetch();

    return {
      articles,
    };
  },
};
</script>

<style lang="scss" scoped>
.intro {
  text-align: center;
  margin-bottom: 2.4rem;

  h1 {
    margin-top: 0;
  }
}
</style>
