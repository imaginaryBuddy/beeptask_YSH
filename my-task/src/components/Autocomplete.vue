
<template>
    <div class="grid absolute justify-center w-48">
        <div class="bg-white px-4 py-2 rounded shadow-sm">
            <input type="text" class="p-2" autocomplete="off" placeholder="🔎 for Asian Countries" v-model="country" @focus="modal=true" @focusout="deactivate"
            @keydown.up.prevent="navigateUsingKeyboard(-1)"
            @keydown.down.prevent="navigateUsingKeyboard(1)"
            @keydown.enter.prevent="selectItem"
            @keydown.esc.prevent="modal=false"
            />   
            <div class="bg-color-gray-100 max-w-48 max-h-20 overflow-y-scroll" v-if="filteredCountries && modal">
            <div
            v-for="(filteredCountry, index) in filteredCountries"
            :key="index"
            class="text-slate-600 py-2 px-1 cursor-pointer"
            :class="{ 'bg-gray-200 rounded': index === selectedCountryIndex }"
            @click="selectItem"
            ref="filteredCountryElements"
            >
            {{ filteredCountry }}
        </div>
        </div>

    </div>
    </div>
</template>

<script>

export default {

    data: function () {
        return {
            country: '',
            modal: false,
            countries: [
            "Afghanistan", "Armenia", "Azerbaijan", "Bahrain", "Bangladesh", "Bhutan", "Brunei", 
            "Cambodia", "China", "Cyprus", "Georgia", "India", "Indonesia", "Iran", "Iraq", 
            "Israel", "Japan", "Jordan", "Kazakhstan", "Kuwait", "Kyrgyzstan", "Laos", 
            "Lebanon", "Malaysia", "Maldives", "Mongolia", "Myanmar (Burma)", "Nepal", 
            "North Korea", "Oman", "Pakistan", "Palestine", "Philippines", "Qatar", 
            "Saudi Arabia", "Singapore", "South Korea", "Sri Lanka", "Syria", "Tajikistan", 
            "Thailand", "Timor-Leste (East Timor)", "Turkey", "Turkmenistan", 
            "United Arab Emirates", "Uzbekistan", "Vietnam", "Yemen"
            ],
            filteredCountries: [],
            selectedCountryIndex: -1,
        }
    },

    mounted(){
        this.filterCountries();
    },

    methods: {
        filterCountries() {
            if (this.filteredCountries.length == 0){
                this.filteredCountries=this.countries; 
                this.selectedCountryIndex = -1; 
            }
            this.filteredCountries = this.countries.filter(c => {
                this.selectedCountryIndex = 0;
                return c.toLowerCase().startsWith(this.country.toLowerCase());
            })

            
        },

        setState(country){
            this.country = country;
            this.modal = false; 
        },
        
        deactivate(){
            setTimeout(() => this.modal = false, 100);
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

        selectItem(){
            if (this.selectedCountryIndex !== -1) {
                const selectedCountry = this.filteredCountries[this.selectedCountryIndex];
                this.setState(selectedCountry);
            }
        }
    },
    
    watch: {
        country(){
            this.filterCountries();
        }
    }
}
</script>