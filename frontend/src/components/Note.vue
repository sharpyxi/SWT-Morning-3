<template>
  <div class="note">
    <div class="note-header">
      <p class="note-title">{{ title }}<span hidden>{{id}}</span></p>
      <p class="note-date">{{ date }}</p>
    </div>
    <div class="note-body">
      <p>{{ description }}</p>
    </div>
    <div class="note-footer">
      <router-link :to="`/edit/${id}`">edit</router-link>&nbsp;
      <a href="#" @click="deleteNote(id)">delete</a>&nbsp;
      <router-link :to="`/share/${id}`">share</router-link>&nbsp;
      <router-link :to="`/export/${id}`">export</router-link>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator'

import moment from 'moment'
import { NoteService } from '@/service/NoteService'

const service = new NoteService()

@Component({})
export default class Note extends Vue {
  @Prop({ default: 0 }) readonly id!: number
  @Prop({ default: '' }) readonly title!: string
  @Prop({ default: '' }) readonly description!: string
  @Prop({ default: new Date(0) }) readonly timestamp!: Date

  get date () {
    return moment(this.timestamp).format('YYYY-MM-DD')
  }

  deleteNote (id: string) {
    service.deleteNote(id)
      .then(() => this.$store.dispatch('sync'))
  }
}
</script>

<style lang="scss">
.note {
  font: 16px/150% sans-serif;

  border: 1px solid #ccc;
  padding: 1rem;

  &:not(first-child) {
    margin-top: 0.5rem;
  }

  & > .note-header {
    display: flex;
    flex-direction: row;
    justify-content: space-between;

    & > p.note-title {
      font-weight: 700;
    }

    & > p.note-date {
      text-align: right;
    }
  }
  & > .note-footer {
    text-align: right;
  }
}

</style>
