<template>
<div class="all-data" :key="`all-data-global-${key}`">
    <div>
    <div v-if="noResults && searchTerm!=''">
        <h3>Sorry!</h3>
        <p>{{searchTerm}} doesn't seem to exist</p>
    </div>
    <div class="results">
        <div v-for="person in Object.keys(m_gifts)" :key="`alldata-${person}`" class="person-wrapper">
            <div class="person">
                <h2 class="person-name">{{ person }}</h2>
                <div class="person-gifts-wrapper">
                    <h3 class="person-gifts-head">Gifts</h3>
                    <div class="items">
                        <div v-for="gift in m_gifts[person]['gifts']" :key="`alldata-${person}-${gift}`" class="person-gifts">
                            {{ gift }}
                        </div>
                    </div>
                </div>
                <div class="person-lost-wrapper">
                    <h3 class="person-lost-head">Lost Items</h3>
                    <div class="items">
                        <div v-for="lost in m_gifts[person]['lost']" :key="`alldata-${person}-${lost}`" class="person-lost">
                            {{ lost }}
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    </div>
</div>
</template>

<script>
export default {
props: {
    mode: {
        type: String,
        default: () => "All"
    },
    searchTerm: {
        type: String
    }
},
methods: {
    checkUnits(val, both) {
        if(Object.keys(this.ogGifts).includes(val)){
            this.m_gifts = {[val]: this.ogGifts[val]}
        } else if(both) {
            this.checkItems(val)
        } else {
            this.noResults = true;
            this.m_gifts = this.ogGifts;
        }
    },
    checkItems(val) {
        this.noResults = true;
        this.m_gifts = {};
        for(const person of Object.keys(this.ogGifts)){
            if(this.ogGifts[person]["gifts"]!=null &&  this.ogGifts[person]["gifts"].includes(val)){
                this.m_gifts = {[person]: this.ogGifts[person]}
                this.noResults = false;
                break;
            } else if(this.ogGifts[person]["lost"]!=null && this.ogGifts[person]["lost"].includes(val)){
                this.m_gifts = {[person]: this.ogGifts[person]}
                this.noResults = false;
                break;
            }
        }
        if(this.noResults) {
            this.m_gifts = this.ogGifts;
        } 
    },
},
data() {
    return {
        ogGifts: require('@/assets/gifts.json'),
        m_gifts: require('@/assets/gifts.json'),
        key: 0,
        noResults: false
    }
},
watch: {
    searchTerm: function (val, oldVal) {
        val = val.trim().toLowerCase();
        if(this.mode == 'Unit') {
            this.checkUnits(val, false)
        } else if(this.mode == 'Item') {
            this.checkItems(val)
        } else {
            this.checkUnits(val, true)
        }
    }
}
}
</script>

<style scoped>

.results {
  margin: 1.5rem auto;
  max-width: 70vw;
  column-gap: 1.5rem;
}

.person-wrapper {
    margin: 1rem;
    padding: 1rem;
    break-inside: avoid;
    background-color: rgb(79, 68, 94);
    border-radius: 4px;
}

.person:first-of-type, .person-wrapper:first-of-type {
    margin-top: 0;
}

.person {
    margin: 1rem auto;
    width: 100%;
    color: rgb(222, 207, 243);
}
.person-name {
    margin-top: 0;
    text-transform: uppercase;
    color: rgb(140, 248, 230);
}
.items {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    width: 100%;
}
.person-gifts, .person-lost {
    margin-right: 0.75rem;
    margin-left: 0.75rem;
    margin-bottom: 0.3rem;
}
.person-gifts-head {
    color: rgb(83, 223, 83);
}
.person-gifts {
    color: lightgreen;
}
.person-lost-head {
    color: rgb(79, 111, 255)
}
.person-lost {
    color: lightskyblue;
}
@media screen and (min-width: 850px) {
    .results {
        column-count: 2;
    }
}

@media screen and (min-width: 1200px) {
    .results {
        column-count: 3;
    }
}
</style>