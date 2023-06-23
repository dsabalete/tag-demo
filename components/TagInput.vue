<template>
  <div>
    <div class="tag-input-container">
      <p class="text-gray-500 mb-4">
        Insert text here. Type '#' and a letter to get suggestions.
      </p>
      <div id="fake-input" contenteditable="true" @input="handleInput"></div>
      <div class="tag-input-dropdown" v-if="showDropdown">
        <ul class="list-none p-0 m-0">
          <li
            class="p-1 cursor-pointer hover:bg-gray-300"
            v-for="suggestion in tagSuggestions"
            :key="suggestion"
            @click="selectSuggestion(suggestion)"
          >
            {{ suggestion }}
          </li>
        </ul>
      </div>
    </div>

    <!-- Remove this and the following div to remove the debug section -->
    <section class="debug mt-24">
      <h1 class="text-xl">
        State of the component
        <span class="text-sm">(only for debugging purposes)</span>
      </h1>
      <p class="p-4 bg-yellow-50">text: {{ text }}</p>
      <div class="debug-container flex flew-row">
        <div class="p-4 bg-green-50 w-1/2">
          <pre>existingTags: {{ existingTags }}</pre>
        </div>
        <div class="p-4 bg-blue-50 w-1/2">
          <pre>tagSuggestions: {{ tagSuggestions }}</pre>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      text: "",
      tags: [
        "liver",
        "pain",
        "right",
        "left",
        "pancreas",
        "kidney",
        "brain",
        "severe_pain",
        "tumour",
        "cancer",
        "MRI",
        "CT",
        "male",
        "female",
        "bone",
        "shoulder",
        "hip",
        "XRAY",
        "knee",
        "spine",
        "head",
        "abdomen",
        "contrast",
        "fragment",
        "detached",
        "injury",
        "torn",
        "rotator",
        "cuff",
        "abdominal",
        "dilatation",
      ], // List of tags available for suggestions
      existingTags: [], // List of existing tags entered by the user
      tagSuggestions: [], // List of tag suggestions based on user input
      showDropdown: false, // Flag to control the visibility of the dropdown
    };
  },
  methods: {
    handleInput(e) {
      this.text = e.target.innerText;
      const tagRegex = /#\w+/g;
      const matches = this.text.match(tagRegex);
      this.existingTags = matches ? matches.map((tag) => tag.slice(1)) : [];
      if (e.data !== " ") {
        this.updateTagSuggestions();
      }
    },

    updateTagSuggestions() {
      const lastWordRegex = /\B#(\w+)$/;
      const lastWordMatches = this.text.match(lastWordRegex);
      const lastWord = lastWordMatches ? lastWordMatches[1] : "";
      this.tagSuggestions = this.tags.filter(
        (tag) =>
          tag.toLowerCase().startsWith(lastWord.toLowerCase()) &&
          !this.existingTags.includes(tag)
      );
      this.showDropdown = lastWord.length > 0 && this.tagSuggestions.length > 0;
    },

    selectSuggestion(suggestion) {
      const lastHashIndex = this.text.lastIndexOf("#");
      const previousText = this.text.slice(0, lastHashIndex);
      this.text = previousText + "#" + suggestion + " ";
      document.getElementById("fake-input").innerText = this.text;
      this.showDropdown = false;
    },
  },
};
</script>

<style lang="postcss" scoped>
.tag-input-container {
  @apply mx-auto w-1/2 mt-2 relative;
}

#fake-input {
  @apply w-full p-2 mb-4 border border-gray-300;
  min-height: 10rem;
}

.tag-input-dropdown {
  @apply absolute w-1/2 max-h-40 bg-gray-100 border border-gray-500 p-1 overflow-y-auto;
}

.debug {
  @apply mx-16;
}
</style>
