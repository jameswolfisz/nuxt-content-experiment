<template>
  <div>
    <ul>
      <li v-for="{ _path: slug, title } in blogPosts" :key="slug">
        <NuxtLink :to="slug">{{ title }}</NuxtLink>
      </li>
    </ul>

    <ContentList :query="query" v-slot="{ list }">
      <ULandingCard
        v-for="article in list"
        :key="article._path"
        :title="article.title"
        :description="article.description"
        orientation="horizontal"
      >
        <img :src="article.image" :alt="article.title" />
      </ULandingCard>

      <div v-for="article in list" :key="article._path">
        <h2>{{ article.title }}</h2>
        <p>{{ article.description }}</p>
      </div>
    </ContentList>

    <pre>
      {{ blogPosts }}
    </pre>
  </div>
</template>

<script setup lang="ts">
import type { QueryBuilderParams } from "@nuxt/content/dist/runtime/types";

const { data: blogPosts } = await useAsyncData(`blog-posts`, () =>
  queryContent("blog")
    .where({ _extension: "md", navigation: { $ne: false } })
    .find()
);

const query: QueryBuilderParams = {
  path: "/blog",
  where: { _extension: "md", navigation: { $ne: false } },
  sort: [{ date: -1 }],
};
</script>
