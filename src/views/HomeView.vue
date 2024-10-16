<script setup>
import { ref, reactive, computed, onMounted } from "vue";

// Define hashtags without the '#' at the start
const artistHashtags = [
  "sketch",
  "painting",
  "draw",
  "instaart",
  "creative",
  "arte",
  "ink",
  "streetart",
  "color",
  "sketchbook",
  "artoftheday",
  "paint",
  "doodle",
  "contemporaryart",
  "pencil",
  "artsy",
  "graffiti",
  "digitalart",
  "abstract",
  "instaartist",
  "watercolor",
  "picture",
  "artistic",
  "arts",
  "gallery",
  "fineart",
  "artgallery",
  "tattoos",
  "graphic",
  "illustrator",
  "sketch",
  "masterpiece",
  "pen",
  "instadraw",
  "paper",
  "arts_help",
  "myart",
  "modernart",
  "sketching",
  "drawings",
  "doodles",
  "doodling",
  "doodlesofinstagram",
  "doodleart",
  "markers",
  "paintings",
  "artshow",
  "myartwork",
  "newartwork",
  "artists",
  "instaartoftheday",
  "instaartwork",
  "sketchaday",
  "colour",
  "instaartistic",
  "instaartsy",
  "instaarts",
  "artinfo",
  "instaarthub",
  "instaarte",
  "mydrawing",
  "instaartpop",
];

const travelHashtags = [
  "travelling",
  "traveler",
  "tourism",
  "travelingram",
  "igtravel",
  "europe",
  "traveller",
  "travelblog",
  "tourist",
  "travelblogger",
  "traveltheworld",
  "roadtrip",
  "instatraveling",
  "instapassport",
  "instago",
  "outdoors",
  "ocean",
  "mytravelgram",
  "traveladdict",
  "world",
  "hiking",
  "lonelyplanet",
  "travelpics",
  "travelbug",
  "travelphoto",
  "travellife",
  "globetrotter",
  "ilovetravel",
  "travelling",
  "traveldiaries",
  "travels",
  "passportready",
  "traveldeeper",
  "getaway",
  "instatravelling",
  "instavacation",
  "travelwriter",
  "wanderer",
  "getoutside",
  "exploremore",
  "outdoor",
  "wander",
  "discover",
  "adventuretime",
];

const foodHashtags = [
  "foodgasm",
  "yum",
  "eat",
  "foodpic",
  "dinner",
  "foodpics",
  "lunch",
  "breakfast",
  "foodphotography",
  "foodlover",
  "tasty",
  "hungry",
  "foodblogger",
  "dessert",
  "healthyfood",
  "chocolate",
  "foods",
  "cooking",
  "cake",
  "eeeeeats",
  "foodies",
  "eating",
  "homemade",
  "chef",
  "foodblog",
  "nomnom",
  "delish",
  "feedfeed",
  "fresh",
  "nom",
  "icecream",
  "foodgram",
  "desserts",
  "brunch",
  "fruit",
  "cheese",
  "f52grams",
  "foodstyling",
  "fooddiary",
  "foodshare",
  "vscofood",
  "foodlove",
  "huffposttaste",
  "foodlovers",
  "buzzfeast",
  "buzzfeedfood",
  "beautifulcuisines",
  "igfood",
  "onthetable",
  "foodphoto",
  "eatfamous",
  "forkyeah",
  "thekitchn",
  "foodandwine",
  "yahoofood",
  "lifeandthyme",
  "eater",
  "heresmyfood",
  "food52",
  "gloobyfood",
  "foodgawker",
  "eattheworld",
  "tastingtable",
  "dailyfoodfeed",
  "spoonfeed",
  "foodbeast",
  "hautecuisines",
  "foodblogfeed",
  "onmytable",
];

const textAreaContent = ref("");
const shuffledHashtags = ref("");
const customHashtags = ref([]);
const hashtagCount = ref(30); // Default to maximum

// Load custom hashtags from local storage on component mount
onMounted(() => {
  const storedHashtags = localStorage.getItem("customHashtags");
  const storedCount = localStorage.getItem('hashtagCount');
  if (storedHashtags) {
    customHashtags.value = JSON.parse(storedHashtags);
    textAreaContent.value = customHashtags.value.map(tag => `#${tag}`).join(' '); // Load hashtags into textarea
  }
  if (storedCount) {
    hashtagCount.value = parseInt(storedCount, 10); // Load hashtag count
    
  }
});

// Fill textarea with hashtags based on the selected category
const fillTextArea = (type) => {
  let hashtags;
  if (type === "artist") {
    hashtags = artistHashtags;
  } else if (type === "travel") {
    hashtags = travelHashtags;
  } else if (type === "food") {
    hashtags = foodHashtags;
  }
  // Set new hashtags in the textarea
  textAreaContent.value = hashtags.map((tag) => `#${tag}`).join(" ") + " ";
};

// Handle input in the textarea
const updateHashtags = (event) => {
  let inputValue = event.target.value;

  // Split input into words
  let words = inputValue.split(" ");

  // Ensure the first word is prefixed with #
  if (words[0] && !words[0].startsWith("#")) {
    words[0] = `#${words[0]}`;
  }

  // Add # for every new word that doesn't start with it
  for (let i = 1; i < words.length; i++) {
    if (words[i] && !words[i].startsWith("#")) {
      words[i] = `#${words[i]}`;
    }
  }

  // Join the words back into a string
  textAreaContent.value = words.join(" ");

  // Update custom hashtags
  updateCustomHashtags();
  saveToLocalStorage(); // Save to local storage
};

// Update the local storage for custom hashtags
const updateCustomHashtags = () => {
  const words = textAreaContent.value.split(" ");
  customHashtags.value = []; // Reset custom hashtags
  words.forEach((word) => {
    if (word.startsWith("#") && word.length > 1) {
      const trimmedHashtag = word.slice(1);
      if (!customHashtags.value.includes(trimmedHashtag)) {
        customHashtags.value.push(trimmedHashtag);
      }
    }
  });
  localStorage.setItem("customHashtags", JSON.stringify(customHashtags.value));
};
// Save current textarea content to local storage
const saveToLocalStorage = () => {
  localStorage.setItem("textareaContent", textAreaContent.value);
  localStorage.setItem('hashtagCount', hashtagCount.value);
};

// Load textarea content from local storage
const loadTextAreaContent = () => {
  const storedContent = localStorage.getItem("textareaContent");
  if (storedContent) {
    textAreaContent.value = storedContent;
  }
};

// Shuffle hashtags based on the specified count
const shuffleHashtags = () => {
  const allHashtags = textAreaContent.value
    .split(" ")
    .filter((word) => word.startsWith("#"));
  const count = Math.min(hashtagCount.value, allHashtags.length);

  const shuffled = allHashtags.sort(() => 0.5 - Math.random()).slice(0, count);
  shuffledHashtags.value = shuffled.join(" ");
};

// Copy the shuffled hashtags to the clipboard
const copyToClipboard = () => {
  if (!shuffledHashtags.value.trim()) {
    alert("No hashtags to copy!");
    return;
  }

  navigator.clipboard
    .writeText(shuffledHashtags.value)
    .then(() => {
      alert("Hashtags copied to clipboard!");
    })
    .catch((err) => {
      console.error("Failed to copy: ", err);
    });
};



// Computed properties for total hashtags and characters
const totalHashtags = computed(() => {
  return textAreaContent.value.split(" ").filter((word) => word.startsWith("#"))
    .length;
});

const totalCharacters = computed(() => {
  return textAreaContent.value.length;
});

// Load content on component mount
loadTextAreaContent();
</script>

<template>
  <main class=" px-2 md:px-[17rem]">
    <section class="md:w-[50%] flex flex-col gap-4">
      <h1 class="text-4xl">Hashtags Shuffler</h1>
      <span>
        Save this page for easy access and revisit it whenever you'd like! This
        site utilizes your browser's local storage to remember your hashtags for
        future visits.
      </span>
    </section>
    <section class="">
      <h1>choose a topic to get started:</h1>
      <div class="flex gap-1 mb-2">
        <button
          @click="fillTextArea('artist')"
          class="bg-blue-300 text-white px-3 py-0.5 text-sm rounded-md outline-none"
        >
          Artist
        </button>
        <button
          @click="fillTextArea('travel')"
          class="bg-blue-300 text-white px-3 py-0.5 text-sm rounded-md outline-none"
        >
          Travel
        </button>
        <button
          @click="fillTextArea('food')"
          class="bg-blue-300 text-white px-3 py-0.5 text-sm rounded-md outline-none"
        >
          Food
        </button>
      </div>
      <div>
        <textarea
          class="w-full outline outline-blue-600 p-2"
          v-model="textAreaContent"
          @input="updateHashtags"
          @blur="handleCustomHashtag"
          name=""
          id=""
          rows="10"
          maxlength="4000"
          spellcheck="false"
          placeholder="put ur own Hashtags here" 
        >
        </textarea>
        <span>Total Hashtags: {{ totalHashtags }} </span>
        <span>Total Characters: {{ totalCharacters }} / 4000 </span>
      </div>
    </section>
    <section class="">
      <div class="flex gap-3 ">
        <h1 class="">No. of hashtags</h1>
        <span>30 maximum </span>
        <input v-model="hashtagCount" @change="saveToLocalStorage" class="border border-1 p-2 border-blue-600" type="number" min="1" max="30" name="shuffle-hashtag" />
      </div>
      <button
        @click="shuffleHashtags"
        class="bg-blue-300 text-white px-3 py-0.5 text-sm rounded-md outline-none"
      >
        Shuffle
      </button>
    </section>
    <section class="mt-2">
      <div class="flex flex-col">
        <textarea
          v-model="shuffledHashtags"
          @click="copyToClipboard"
          class="w-full outline outline-blue-600 p-2"
          readonly
          name="shuffled-hashtags"
          id="shuffled-hashtags"
          rows="10"
          placeholder="click the shuffle button and click the textarea to copy"
        >
        </textarea>
      </div>
    </section>
  </main>
</template>
