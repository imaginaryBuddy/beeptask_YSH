
<template>
  <div class="grid grid-rows-2 absolute justify-center w-48">
    <div class="bg-white px-4 py-2 rounded shadow-sm">
      <input
        :disabled="isDisabled"
        :class="{'bg-red-100 rounded': isDisabled}"
        type="text" 
        class="p-2"
        autocomplete="off"
        :placeholder="isDisabled? 'disabled' : '🔎 (async)'"
        v-model="country"
        @focus="onFocus"
        @focusout="deactivate"
        @keydown.up.prevent="navigateUsingKeyboard(-1)"
        @keydown.down.prevent="navigateUsingKeyboard(1)"
        @keydown.enter.prevent="selectItem(this.selectedCountryIndex)"
        @keydown.esc.prevent="modal=false"
      />
      
      <div class="bg-color-gray-100 max-w-48 max-h-20 overflow-y-scroll" v-if="modal">
        <IsLoadingSpinner class="z-99 mt-2" v-if="isLoading" />
        <div v-else>
          <div v-for="(filteredCountry, index) in filteredCountries" 
          :key="index"
          class="text-slate-600 py-2 cursor-pointer" 
          :class="{ 'bg-gray-200 rounded': index === this.selectedCountryIndex }"
          @click="selectItem(index)"
          >
            {{ filteredCountry.name }}
            {{ filteredCountry.emoji }}
          </div>
        </div>
      </div>
    </div>
    <div class="col-span-1 h-10 w-10 bg-purple-200 rounded mt-3 ml-44">
      <button class="items-center justify-center h-full w-full text-xs" @click="toggleDisable">{{isDisabled? "✍️" : "❌✍️"}}</button>
    </div>
  </div>
</template>


<script>
import IsLoadingSpinner from "./IsLoadingSpinner.vue";
import ToggleButton from "./ToggleButton.vue";

export default {
  data() {
    return {
      isLoading: false,
      country: "",
      filteredCountries: [],
      modal: false,
      debounceTime: null,
      selectedCountryIndex: -1,
      isDisabled: false,
    };
  },

  components: {
    IsLoadingSpinner,
    ToggleButton,
  },

  methods: {
    deactivate() {
      setTimeout(() => (this.modal = false), 100);
    },

    setState(country) {
      this.country = country.name + country.emoji;
      this.modal = false;
    },

    onFocus(){
      this.modal = true; 
      this.filterCountries();
    },

    filterCountries() {

      this.isLoading = true;

      fetch(`http://localhost:3000/countries`)
        .then((res) => res.json())
        .then((data) => {
            if (this.country.trim() == ""){
              this.filteredCountries = data;
            } else {
              this.filteredCountries = data.filter((d) => {
            
                if(d.name.toLowerCase().startsWith(this.country.toLowerCase())){
                  return d;
                }
              })
        
            }
          
       

        })
        .catch((error) => {
          console.error("Error fetching countries", error);
        })
        .finally(() => {
            setTimeout(() => {
                    this.isLoading = false; 
                }, 1000);
        });
    },

    selectItem(index){
        this.selectedCountryIndex = index; 
        const selectedCountry = this.filteredCountries[this.selectedCountryIndex];
        this.setState(selectedCountry);

    },

    navigateUsingKeyboard(direction){
            const newIndex = this.selectedCountryIndex + direction; 

            if (newIndex >= 0 && newIndex < this.filteredCountries.length){
                this.selectedCountryIndex = newIndex; 
                const element = this.$refs.filteredCountryElements[newIndex];
                this.scrollIntoView(element); 
            }
        },

        scrollIntoView(element){
            if(element){
                element.scrollIntoView({behavior: "smooth", block: "nearest"})
            }
        },

        toggleDisable() {
          this.isDisabled = !this.isDisabled;
        },
  },
  watch: {
    country() {
      clearTimeout(this.debounceTimer);
      this.debounceTimer = setTimeout(()=> this.filterCountries(), 500);
    },
  }
}
</script>
