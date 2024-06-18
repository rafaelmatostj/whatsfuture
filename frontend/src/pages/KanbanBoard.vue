<!-- frontend/src/pages/KanbanBoard.vue -->
<template>
    <div>
      <kanban-board :stages="board" @add="addCard" @move="moveCard"></kanban-board>
    </div>
  </template>
  
  <script>
  import { KanbanBoard } from '@asseinfo/vue-kanban'
  import '@asseinfo/vue-kanban/dist/vue-kanban.css'
  
  export default {
    components: {
      KanbanBoard
    },
    data() {
      return {
        board: [
          {
            id: 1,
            name: 'To Do',
            items: []
          },
          {
            id: 2,
            name: 'In Progress',
            items: []
          },
          {
            id: 3,
            name: 'Done',
            items: []
          }
        ]
      }
    },
    methods: {
      fetchConversations() {
        fetch('/api/conversations')
          .then(response => response.json())
          .then(data => {
            this.board[0].items = data.map(convo => ({
              id: convo.id,
              title: convo.title,
              description: convo.message
            }))
          })
      },
      addCard(stageId, newCard) {
        const stage = this.board.find(stage => stage.id === stageId)
        stage.items.push(newCard)
      },
      moveCard({ fromStageId, toStageId, item }) {
        const fromStage = this.board.find(stage => stage.id === fromStageId)
        const toStage = this.board.find(stage => stage.id === toStageId)
        fromStage.items = fromStage.items.filter(i => i.id !== item.id)
        toStage.items.push(item)
      }
    },
    created() {
      this.fetchConversations()
    }
  }
  </script>
  
  <style scoped>
  /* Adicione estilos personalizados, se necessário */
  </style>
  