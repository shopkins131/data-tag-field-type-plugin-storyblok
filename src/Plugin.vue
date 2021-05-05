<template>
    <div>
      <label class="form__topic">Field Title: {{ title }}</label>
      <multiselect 
        v-model="value" 
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
      <pre class="language-json"><code>{{ value }}</code></pre>
    </div>
</template>

<script>
import Multiselect from "vue-multiselect";
import options from "./options.json";

export default {
  mixins: [window.Storyblok.plugin],
  data() {
    return {
      value: [],
      options,
      title: "Countries",
      description: "Data tags representing the countries",
    };
  },
  methods: {
    initWith() {
      return {
        // needs to be equal to your storyblok plugin name
        plugin: "data-tags",
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
      this.options.push(tag);
      this.value.push(tag);
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

.uk-form {
  height: unset;
}

.multiselect__tags {
  padding: 0px;
  padding-right: 40px;
  border: none;
}

.multiselect__tag {
  background: #09b3af;
  margin-right: 0;
}

.multiselect__placeholder {
  display: none
}

.multiselect__tags  input {
    width: 100% !important;
    padding: 10px !important;
    height: 38px !important;
    position: static !important;
}

.multiselect__content-wrapper {
  max-height: unset !important;
  max-width: 308px !important;
  overflow: hidden;
}

.multiselect__option {
  font-weight: 400;
  font-size: 14px;
  max-width: 308px;
}

</style>
