<script>
  import TodoItem from './components/TodoItem/TodoItem.svelte';
  import { writable } from 'svelte/store';
  import newUniqueId from 'locally-unique-id-generator';

  const todoList = writable(JSON.parse(localStorage.getItem('todoList') || '[]'));
  
  let lst = []; // #Reactive

  todoList.subscribe((value) => {
    localStorage.todoList = JSON.stringify(value);
    lst = value;
  });

  let text = "";  // #Reactive

  function add() {
    // #controlFlow
    if (text) {
      todoList.update(value => [...value, { id: newUniqueId(), text, complete: false, created: Date.now()}]);
      text = '';
    }
  }

  function updateStatus(event) {
    const { id, newStatus } = event.detail;
    todoList.update(arr => {
      const i = arr.findIndex(value => value.id == id);
      arr[i] = { ...arr[i], complete: newStatus };
      return arr;
    });
  }

  function removeItem(event) {
    todoList.update(arr => {
      const i = arr.findIndex(value => value.id == event.detail.id);
      arr.splice(i, 1);
      return arr;
    });
  }
</script>

<style>
  input { display: block }
  body {
    background-color: #edfff2;
    margin: 0;
    min-height: 100vh;
  }
  .addTodo {
    display: flex;
    flex-direction: row;
    width: 260px;
    margin-bottom: 20px;
    justify-content: space-between;
    align-items: flex-start;
  }
  .todoMain {
    display: flex;
    flex-direction: row;
    width: 800px;
    justify-content: space-between;
    align-items: flex-start;
    margin: auto;
    align-self: center;
  }
  .main {
    margin: auto;
    align-self: center;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .column {
    min-width: 400px;
    max-width: 400px;
    flex: 1;
    min-height: 500px;
    border-width: 2px;
    border-color: #556785;
    border-style: solid;
    border-radius: 5px;
  }
  .column.columnRight {
    border-left-width: 0px;
  }
  .listTitle {
    font-weight: 400;
    text-align: center;
    text-decoration: underline;
  }
</style>

<body>
  <div class="main">
    <h1>Todo</h1>
    <div class="addTodo">
        <!-- #Component #Properties -->
        <input bind:value={text} placeholder="Enter task">
        <!-- #Component #Properties #EventHandler -->
        <button on:click={add}>Add</button>
    </div>
    <div class="todoMain">
      <div class="column">
        <h2 class="listTitle">In Progress</h2>
        <ul>
          <!-- #controlFlow -->
          {#each lst.filter(value => !value.complete) as todo}
            <!-- #Component #Properties #EventHandler -->
            <TodoItem {...todo} on:remove={removeItem} on:toggle={updateStatus} />
                
          {/each}
        </ul>
      </div>
      <div class="column columnRight">
        <h2 class="listTitle">Completed</h2>
        <ul>
          <!-- #controlFlow -->
          {#each lst.filter(value => value.complete) as todo}
            <!-- #Component #Properties #EventHandler -->
            <TodoItem {...todo} on:remove={removeItem} on:toggle={updateStatus} />
              
          {/each}
        </ul>
      </div>
    </div>
  </div>
</body>