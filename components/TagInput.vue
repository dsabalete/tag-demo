<template>
  <div>
    <div class="mx-auto w-1/2 mt-2 relative">
      <div
        id="fake-input"
        class="w-full h-24 p-2 mb-4 border border-gray-300"
        contenteditable="true"
        @input="handleInput"
      ></div>
      <div
        class="absolute w-1/2 max-h-40 bg-gray-100 border border-gray-500 p-1 overflow-y-auto"
        v-if="showDropdown"
      >
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
    <div class="debug mt-64">
      <h1>Debug</h1>
      <pre>text: {{ text }}</pre>
      <pre>existingTags: {{ existingTags }}</pre>
      <pre>tagSuggestions: {{ tagSuggestions }}</pre>
    </div>
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