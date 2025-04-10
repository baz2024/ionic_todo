# Ionic + React Todo App (Single Page)

## 🧼 Clean Setup

If you created your app using:

```bash
ionic start ionic-todo-app blank --type=react
```
 
---

## 🧱 Folder Structure

```text
src/
|- pages/
|   |- TodoPage.tsx
|- App.tsx
|- main.tsx
```

---

## 📄 `pages/TodoPage.tsx`

```tsx
import {
  IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonInput,
  IonButton, IonList, IonItem, IonLabel, IonIcon, IonCheckbox
} from '@ionic/react';
import { trashOutline } from 'ionicons/icons';
import React, { useState, useEffect } from 'react';

const TodoPage: React.FC = () => {
  const [input, setInput] = useState('');
  const [todos, setTodos] = useState<{ text: string, completed: boolean }[]>([]);

  useEffect(() => {
    const stored = localStorage.getItem('todos');
    if (stored) setTodos(JSON.parse(stored));
  }, []);

  useEffect(() => {
    localStorage.setItem('todos', JSON.stringify(todos));
  }, [todos]);

  const handleAddTodo = () => {
    if (!input.trim()) return;
    setTodos([...todos, { text: input, completed: false }]);
    setInput('');
  };

  const handleDeleteTodo = (index: number) => {
    setTodos(todos.filter((_, i) => i !== index));
  };

  const toggleTodo = (index: number) => {
    setTodos(todos.map((todo, i) => i === index
      ? { ...todo, completed: !todo.completed }
      : todo
    ));
  };

  return (
    <IonPage>
      <IonHeader>
        <IonToolbar>
          <IonTitle>Ionic Todo App</IonTitle>
        </IonToolbar>
      </IonHeader>
      <IonContent className="ion-padding">
        <IonInput
          value={input}
          placeholder="Enter a new todo"
          onIonChange={e => setInput(e.detail.value!)}
        />
        <IonButton expand="block" onClick={handleAddTodo} className="ion-margin-top">
          Add
        </IonButton>
        <IonList>
          {todos.map((todo, index) => (
            <IonItem key={index} style={{
              textDecoration: todo.completed ? 'line-through' : 'none',
              backgroundColor: todo.completed ? '#e0f7fa' : ''
            }}>
              <IonCheckbox
                slot="start"
                checked={todo.completed}
                onIonChange={() => toggleTodo(index)}
              />
              <IonLabel>{todo.text}</IonLabel>
              <IonButton fill="clear" slot="end" onClick={() => handleDeleteTodo(index)}>
                <IonIcon icon={trashOutline} />
              </IonButton>
            </IonItem>
          ))}
        </IonList>
      </IonContent>
    </IonPage>
  );
};

export default TodoPage;
```

---

## ⚙️ `App.tsx`

```tsx
import { IonApp, IonRouterOutlet } from '@ionic/react';
import { IonReactRouter } from '@ionic/react-router';
import { Route, Redirect } from 'react-router-dom';
import TodoPage from './pages/TodoPage';

const App: React.FC = () => {
  return (
    <IonApp>
      <IonReactRouter>
        <IonRouterOutlet>
          <Route exact path="/" component={TodoPage} />
          <Redirect to="/" />
        </IonRouterOutlet>
      </IonReactRouter>
    </IonApp>
  );
};

export default App;
```

---

## ✅ Summary

| Feature       | Implemented |
|---------------|-------------|
| Add Todo      | ✅ with `IonInput` & `IonButton` |
| Toggle Done   | ✅ using `IonCheckbox` |
| Delete Todo   | ✅ with trash icon |
| LocalStorage  | ✅ using `useEffect` |
| UI Highlight  | ✅ done todos are styled differently |
