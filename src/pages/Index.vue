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
          <q-link @click.prevent="onRemove(index)">
            <q-icon name="remove"></q-icon>
          </q-link>
        </q-item>
      </q-list>

      <q-btn @click="deryban"  class="q-mt-md" :disabled=" ! canDeryban" color="primary" icon="published_with_changes" :label="$t('deryban')" />
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';
import { ref } from 'vue'


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
    }
  },

  computed: {
    canDeryban() {
      return this.members.length && this.members.length % 2 == 0;
    }
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
      alert('1')
    }
  }
})
</script>
