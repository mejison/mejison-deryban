<template>
  <q-page class="flex column q-pa-md">
      <q-input bottom-slots v-model="memberName" require :label="$t('name_member_placeholder')" counter maxlength="12">
        <template v-slot:before>
          <q-icon name="perm_identity" />
        </template>

        <template v-slot:append>
          <q-btn @click="addMember" round dense flat icon="add" />
        </template>
      </q-input>

      <q-list bordered class="q-mt-md" v-if="members.length">
        <q-item clickable v-ripple v-for="(member, index) in members" :key="index">
          <q-item-section avatar>
            <q-avatar>
              <img src="~/assets/boy-avatar.png">
            </q-avatar>
          </q-item-section>
          <q-item-section>{{ member.name }}</q-item-section>
          <q-btn flat @click.prevent="onRemove(index)"  icon="delete" />
        </q-item>
      </q-list>
      
      <div class="q-ma-md">
        <div class="text-center">
          {{ $t('chunk_by') }}: {{ perChunk }}
        </div>

        <q-slider
          v-model="perChunk"
          :label-always="true"
          :min="sliderOptions.min"
          :max="sliderOptions.max"
        />
      </div>

      <q-btn @click="deryban"  class="q-mt-md q-mb-md" :disabled=" ! canDeryban" color="primary" icon="published_with_changes" :label="$t('deryban')" />

      <div v-if="teams.length">
         <q-list padding bordered class="rounded-borders">
            <q-expansion-item
              v-for="(team, index) in teams" :key="index"
              dense
              dense-toggle
              expand-separator
              icon="supervisor_account"
              :label="`Команда ${ index + 1}: ${team.name}`"
              default-opened
            >
              <q-list  class="q-mt-md" v-if="team.members.length">
                <q-item clickable v-ripple v-for="(member, index) in team.members" :key="index">
                  <q-item-section avatar>
                    <q-avatar>
                      <img src="~/assets/boy-avatar.png">
                    </q-avatar>
                  </q-item-section>
                  <q-item-section>{{ member.name }}</q-item-section>
                </q-item>
              </q-list>
            </q-expansion-item>
          </q-list>
      </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';

import animals from "../animals.json";
import colors from '../colors.json';

export default defineComponent({
  name: 'PageIndex',

  data() {
    return {
      members: [
        {
          name: 'Alex Kovalchuk'
        },
        {
          name: 'Max Shostak'
        }
      ],
      memberName: '',
      perChunk: 2,
      teams: [],
      sliderOptions: {
        min: 1,
        max: 5,
      },
      lang: this.$i18n.locale
    }
  },

  computed: {
    canDeryban() {
      let evenMembers = this.members.length && this.members.length % 2 == 0;
      let chunkLessMembers = this.perChunk < this.members.length;
      let canSeparete = this.members.length % this.perChunk == 0;

      return evenMembers && chunkLessMembers && canSeparete
    },
  },

  methods: {
    onRemove(deleteIndex) {
      this.members = this.members.filter((member, index) => index != deleteIndex)
    },
    addMember() {
      if (this.memberName) {
        this.members = [
          ...this.members,
          {
            name: this.memberName,
          }
        ]
  
        this.memberName = ''
      }
    },
    deryban() {
      if (this.perChunk < this.members.length) {
        this.teams = this.chunk(this.shuffle([...this.members]), this.perChunk)
        this.teams = this.teams.map((members, index) => {
          let randomColor = colors[this.lang][Math.floor(Math.random() * colors[this.lang].length)];
          let randomAnimal = animals[this.lang][Math.floor(Math.random() * animals[this.lang].length)];
          return {
            name: `${randomColor} ${randomAnimal}`,
            members,
          };
        });
      }
    },
    shuffle(array) {
      let currentIndex = array.length,  randomIndex;
      while (currentIndex != 0) {
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;
        [array[currentIndex], array[randomIndex]] = [
          array[randomIndex], array[currentIndex]];
      }

      return array;
    },
    chunk(array, perChunk) {
      return array.reduce((all, one, i) => {
        const ch = Math.floor(i / perChunk); 
        all[ch] = [].concat((all[ch]||[]),one); 
        return all
      }, [])
    },
  }
})
</script>
