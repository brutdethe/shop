<script>
  import { goto, stores } from "@sapper/app";
  import { loadCategories, categorySelected } from "../stores.js";

  export let categories;
  export let lang;

  const { page } = stores();
  const { path, query } = $page;

  if (query.categorie) {
    categorySelected.set(query.categorie);
  }

  let categoriesWording = loadCategories();

  function changeCategory(evt) {
    categorySelected.set(evt.currentTarget.value);
    goto(`${path}?categorie=${$categorySelected}`);
  }

  const isCategoryExists = (categories, categorySelected) =>
    categories.indexOf(categorySelected) !== -1;
</script>

<style>
  .hero-body {
    padding: 0.8rem;
    margin-bottom: 0.8rem;
    box-shadow: 0 0.25rem 1rem rgba(48, 55, 66, 0.15);
  }

  hr {
    color: #ddd;
  }

  .btn-list {
    margin: -10px
  }

  .btn {
    margin: 10px;
    box-shadow: 0 0.25rem 1rem rgba(48, 55, 66, 0.15);
  }
</style>

{#if categories}
  {isCategoryExists(categories, $categorySelected) ? '' : goto('/404')}
  <div class="hero-sm bg-primary">
    <div class="hero-body">
      <h3>{$categorySelected}</h3>
      {#if $categoriesWording[$categorySelected]}
        <p>{$categoriesWording[$categorySelected].description[lang]}</p>
      {/if}
    </div>
  </div>

  {#if categories.length > 1}
    <div class="btn-list">
        {#each categories as category}
          <button
            class="btn btn-sm {category === $categorySelected ? 'btn-primary' : ''}"
            on:click={changeCategory}
            value={category}>
            {#if $categoriesWording[category]}
              {$categoriesWording[category].titre[lang]}
            {/if}
          </button>
        {/each}
    </div>
  {/if}
{/if}

<hr />
