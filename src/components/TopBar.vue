<template>
  <nav class="main-navigation">

        <div class="logo">
            <img src="https://fontmeme.com/permalink/210513/7c5aa84277dd26e22b91885eb4523481.png" alt="img">
        </div>

    <form>
        <input type="text" 
            placeholder="Search" 
            v-model.trim="search"
        >
        <button 
            @click.prevent="$emit('search', search)"
            class="search-button"
        >
        Search
        </button>
    </form>
  </nav>
</template>

<script>


export default {
    name: "TopBar",
    data() {
        return {
            search: "",
        }
    },
}
</script>

<style scoped lang="scss">
@import "@/styles/variables.scss";
@import "@/styles/mixins.scss";


.main-navigation {
    background-color: $main-bg-col;
    @include center("vertical");
    flex-direction: row;
    justify-content: space-between;

    .logo {
        img {
            width: 200px;
            padding: 10px;
        }
    }

    form {
        margin-right: 100px;

        input {
            border: none;
            padding: 5px;
        }

        button {
            margin: 0 10px;
            padding: 5px;
            color: $main-text-col;
            background-color: $button-col;
            border: none;
            cursor: pointer;
        }
    }
}

</style>