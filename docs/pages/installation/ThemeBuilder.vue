<template>
  <div>
    <h1 class="title is-danger">Theme builder</h1>
    <div class="media">
      <div class="media-left">
        <h3 class="subtitle">1</h3>
      </div>
      <div class="media-content">
        <h3 class="subtitle">Instructions</h3>
        <div class="content">
          <ol>
            <li>Select the component you want to customize</li>
            <li>Update the variables</li>
            <li>Check the live updates on the component</li>
            <li>Click "Export Theme"</li>
          </ol>
        </div>
      </div>
    </div>

    <div class="media">
      <div class="media-left">
        <h3 class="subtitle">2</h3>
      </div>
      <div class="media-content">
        <h3 class="subtitle">Components to customize</h3>
        <b-tabs :vertical="true">
          <b-tab-item
            v-for="tab in normalizeTabs"
            :label="tab.title"
            :icon="tab.icon"
            :key="tab.title"
            :disabled="tab.disabled"
          >
            <slot>
              <h4 class="subtitle">{{tab.title}}</h4>
              <component v-if="!tab.disabled" v-bind:is="tab.component" />
            </slot>
          </b-tab-item>
        </b-tabs>
      </div>
    </div>
  </div>
</template>

<script>
import Tabs from "@/data/themebuildermenu";

export default {
  components: {
    Button: () => import("../../components/themebuilder/button"),
    Colors: () => import("../../components/themebuilder/colors")
  },
  data() {
    return {
      tabSelected: 0,
      variables: []
    };
  },
  computed: {
    normalizeTabs: {
      get: function() {
        return Tabs.tabs.map(t => {
          return {
            title: this.capitalize(t.title),
            disabled: t.disabled === true,
            icon: t.icon,
            component: this.capitalize(t.component)
          };
        });
      }
    }
  },
  methods: {
    /**
     * Stupidly capitalize the first letter of a text
     */
    capitalize: function(text) {
      return text.charAt(0).toUpperCase() + text.slice(1);
    }
  }
};
</script>