<template>
    <div>
      <multiselect 
        v-model="model.value" 
        :height="600"
        :options="model.options" 
        :multiple="true" 
        :taggable="true"
        :close-on-select="false" 
        :clear-on-select="false" 
        :preserve-search="true" 
        tag-placeholder="Add this as new tag" 
        placeholder="Search or add a tag"
        @tag="addTag"
        label="tag_name" 
        track-by="tag_name" 
        :preselect-first="false"
      />
    </div>
</template>

<script>
import Multiselect from "vue-multiselect";

export default {
  mixins: [window.Storyblok.plugin],
  methods: {
    initWith() {
      return {
        // needs to be equal to your storyblok plugin name
        plugin: "data-tags",
        value: [],
        options: []
      };
    },
    pluginCreated() {
      // eslint-disable-next-line
      console.log("plugin ready");
    },
    addTag(newTag) {
      // eslint-disable-next-line
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
  mounted() {
    const params = {
      starts_with: `data-tag-lib/${this.options.category}`,
      token: this.options.token
    }
    this.api.get("cdn/stories", params)
      .then(response => {
        this.model.options = response.data.stories[0].content.options;
      })
      .catch(error => {
        alert(error);
      });
  }
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
