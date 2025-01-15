<template>
  <section class="wrapper py-4">
    <header class="text-center mb-4">
      <h1 class="app-title">Заметки</h1>
    </header>

    <form @submit.prevent="handleSubmit" class="note-form p-4 mb-5">
      <div class="form-group mb-3">
        <label for="noteContent" class="label">Тут что-то пишем</label>
        <textarea id="noteContent" v-model="currentNote.content" class="input" rows="4" required></textarea>
      </div>

      <div class="form-group mb-3">
        <span class="label">Чек-боксы</span>
        <div class="radio-group">
          <label class="radio-label">
            <input type="radio" value="left" v-model="currentNote.priority" /> Лево
          </label>
          <label class="radio-label">
            <input type="radio" value="center" v-model="currentNote.priority" /> Центр
          </label>
          <label class="radio-label">
            <input type="radio" value="right" v-model="currentNote.priority" /> Право
          </label>
        </div>
      </div>

      <div class="form-group mb-3">
        <label for="categorySelect" class="label">Тут что-то выбираем</label>
        <select id="categorySelect" v-model="currentNote.category" class="dropdown">
          <option value="to">То</option>
          <option value="sio">Се</option>
          <option value="fifth">Пятое</option>
          <option value="tenth">Десятое</option>
        </select>
      </div>

      <div class="form-check mb-3">
        <input type="checkbox" id="completedCheck" v-model="currentNote.isDone" class="checkbox" />
        <label for="completedCheck" class="label">Тут соглашаемся</label>
      </div>

      <button type="submit" class="btn-submit">{{ isEditing ? "Обновить заметку" : "Добавить заметку" }}</button>
    </form>

    <section v-if="notes.length" class="notes-section">
      <h2 class="section-title">Ваши заметки</h2>
      <div class="note-card" v-for="(note, idx) in notes" :key="idx">
        <header class="note-header">
          <h3 class="note-category">{{ note.category }} - {{ note.priority }}</h3>
          <span class="note-status" :class="{'done': note.isDone}">{{ note.isDone ? 'Согласны' : 'Не согласны' }}</span>
        </header>
        <p class="note-content">{{ note.content }}</p>
        <div class="note-actions">
          <button @click="editNote(idx)" class="btn-edit">Изменить заметку</button>
          <button @click="removeNote(idx)" class="btn-delete">Удалить заметку</button>
        </div>
      </div>
    </section>
    <p v-else class="empty-message">Пока пусто</p>
  </section>
</template>

<script>
export default {
  data() {
    return {
      currentNote: {
        content: '',
        priority: 'Low',
        category: 'Work',
        isDone: false
      },
      notes: [],
      isEditing: false,
      editIndex: null
    };
  },
  methods: {
    handleSubmit() {
      if (this.isEditing) {
        this.notes.splice(this.editIndex, 1, { ...this.currentNote });
        this.isEditing = false;
        this.editIndex = null;
      } else {
        this.notes.push({ ...this.currentNote });
      }
      this.resetForm();
    },
    editNote(index) {
      this.currentNote = { ...this.notes[index] };
      this.isEditing = true;
      this.editIndex = index;
    },
    removeNote(index) {
      this.notes.splice(index, 1);
    },
    resetForm() {
      this.currentNote = {
        content: '',
        priority: 'Low',
        category: 'Work',
        isDone: false
      };
    }
  }
};
</script>

<style scoped>
body {
  background-color: #e8f5e9;
  font-family: 'Verdana', sans-serif;
  margin: 0;
  color: #333;
}

.wrapper {
  max-width: 700px;
  margin: auto;
  padding: 20px;
}

.app-title {
  font-size: 2.5rem;
  color: #000000;
  font-weight: bold;
}

.note-form {
  background: #ffffff;
  border: 1px solid #ddd;
  border-radius: 8px;
}

.label {
  font-size: 1rem;
  font-weight: bold;
  color: #000000;
}

.input, .dropdown {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.checkbox {
  margin-right: 10px;
}

.btn-submit {
  background-color: #388e3c;
  color: #fff;
  border: none;
  padding: 10px;
  border-radius: 5px;
  cursor: pointer;
}

.btn-submit:hover {
  background-color: #84b087;
}

.notes-section {
  margin-top: 20px;
}

.note-card {
  background: #fff;
  padding: 15px;
  margin-bottom: 15px;
  border-radius: 8px;
  border: 1px solid #ccc;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.note-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.note-category {
  font-size: 1.2rem;
  color: #388e3c;
  font-weight: bold;
}

.note-status {
  font-size: 0.9rem;
  font-weight: bold;
}

.note-status.done {
  color: #000000;
}

.note-content {
  margin: 10px 0;
  font-size: 1rem;
  color: #555;
}

.note-actions {
  display: flex;
  gap: 10px;
}

.btn-edit {
  background: #388e3c;
  color: #fff;
  border: none;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
}

.btn-edit:hover {
  background: #388e3c;
}

.btn-delete {
  background: #d32f2f;
  color: #fff;
  border: none;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
}

.btn-delete:hover {
  background: #c62828;
}

.empty-message {
  font-size: 1rem;
  color: #777;
  text-align: center;
}
</style>
