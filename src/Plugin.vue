<template>
    <div>
      <multiselect 
        v-model="model.value" 
        :height="600"
        :options="options" 
        :multiple="true" 
        :taggable="true"
        :close-on-select="false" 
        :clear-on-select="false" 
        :preserve-search="true" 
        tag-placeholder="Add this as new tag" 
        placeholder="Search or add a tag"
        @tag="addTag"
        label="title" 
        track-by="title" 
        :preselect-first="false"
      />
      <span class="uk-text-muted form__hint">{{ description }}</span>

    </div>
</template>

<script>
import Multiselect from "vue-multiselect";
import options from "./options.json";

export default {
  mixins: [window.Storyblok.plugin],
  data() {
    return {
      options,
      description: "Data tags representing the countries",
    };
  },
  methods: {
    initWith() {
      return {
        // needs to be equal to your storyblok plugin name
        plugin: "data-tags",
        value: [],
      };
    },
    pluginCreated() {
      // eslint-disable-next-line
      console.log("plugin ready");
    },
    addTag(newTag) {
      const tag = {
        title: newTag,
        // you'll need to add other items specific to your objects
      };
      this.model.value.push(tag);
    },
  },
  components: {
    Multiselect,
  },
  watch: {
    model: {
      handler: function(value) {
        this.$emit("changed-model", value);
      },
      deep: true,
    },
  },
};
</script>

<style>
@import "https://unpkg.com/vue-multiselect@2.1.0/dist/vue-multiselect.min.css";

.multiselect--active {
  position: static;
  z-index: unset;
}

.multiselect__tags {
  padding: 0px;
  padding-right: 40px;
  border: 1px solid #D5D5D5;
  border-radius: 0;
}

.multiselect__tag:first-of-type {
  margin-top: 5px;
}

.multiselect__tag {
  background: #09b3af;
  margin-right: 0;
  margin-left: 5px;
}

.multiselect__placeholder {
  display: none
}

.multiselect__tags  input {
  width: 100% !important;
  padding: 10px !important;
  height: 40px !important;
  position: static !important;
  border: none !important;
}

.multiselect__tags  input:focus {
  border: 1px solid #09b3af !important;
}

.multiselect__content-wrapper {
  max-height: unset !important;
  max-width: 308px !important;
  overflow-x: hidden;
  position: static;
  z-index: unset;
}

.multiselect__option {
  font-weight: 400;
  font-size: 14px;
  max-width: 308px;
}

</style>
