# Advanced Ionic + React Tutorial
### Author: Dr. Basel Magableh
### Date: April 2025

## Overview
This tutorial demonstrates how to build an advanced Ionic app using React. It includes:

- Login and logout functionality
- A side menu (drawer-style navigation)
- Avatar with popover
- Header-based breadcrumbs
- Cards for user interface sections
- Data listing with IonList

### Step 1: Create the Project
Install the Ionic CLI and create a React-based Ionic app:

```bash
npm install -g @ionic/cli
ionic start ionic-todo-app tabs --type=react
cd ionic-todo-app
```

### Step 2: Folder Structure
Recommended folder layout:

```text
src/
|- components/
|  |- Header.tsx
|- pages/
|  |- Login.tsx
|  |- Dashboard.tsx
|- App.tsx
```

### Step 3: Login Page
`pages/Login.tsx`:

```tsx
import { IonContent, IonPage, IonInput, IonButton, IonItem, IonLabel } from '@ionic/react';
import { useHistory } from 'react-router';

const Login: React.FC = () => {
  const history = useHistory();
  const handleLogin = () => {
    localStorage.setItem('user', 'demo');
    history.push('/dashboard');
  };

  return (
    <IonPage>
      <IonContent className="ion-padding">
        <IonItem>
          <IonLabel position="floating">Username</IonLabel>
          <IonInput />
        </IonItem>
        <IonButton expand="full" onClick={handleLogin}>Login</IonButton>
      </IonContent>
    </IonPage>
  );
};

export default Login;
```

### Step 4: Header with Avatar + Popover
`components/Header.tsx`:

```tsx
import {
  IonHeader, IonToolbar, IonTitle, IonButtons,
  IonAvatar, IonPopover, IonContent, IonItem
} from '@ionic/react';
import React, { useState } from 'react';

const Header: React.FC = () => {
  const [showPopover, setShowPopover] = useState(false);

  return (
    <>
      <IonHeader>
        <IonToolbar>
          <IonTitle>Ionic Dashboard</IonTitle>
          <IonButtons slot="end">
            <IonAvatar onClick={() => setShowPopover(true)}>
              <img alt="avatar" src="https://i.pravatar.cc/40" />
            </IonAvatar>
          </IonButtons>
        </IonToolbar>
      </IonHeader>
      <IonPopover
        isOpen={showPopover}
        onDidDismiss={() => setShowPopover(false)}
      >
        <IonContent className="ion-padding">
          <IonItem button onClick={() => {
            localStorage.clear();
            window.location.href = "/";
          }}>Logout</IonItem>
        </IonContent>
      </IonPopover>
    </>
  );
};

export default Header;
```

### Step 5: Dashboard with Card and List
`pages/Dashboard.tsx`:

```tsx
import {
  IonPage, IonContent, IonCard, IonCardHeader,
  IonCardTitle, IonCardContent, IonList, IonItem
} from '@ionic/react';
import Header from '../components/Header';

const Dashboard: React.FC = () => {
  const tasks = ['Buy groceries', 'Read book', 'Clean kitchen'];

  return (
    <IonPage>
      <Header />
      <IonContent className="ion-padding">
        <IonCard>
          <IonCardHeader>
            <IonCardTitle>Welcome</IonCardTitle>
          </IonCardHeader>
          <IonCardContent>Here are your tasks:</IonCardContent>
        </IonCard>
        <IonList>
          {tasks.map((task, idx) => (
            <IonItem key={idx}>{task}</IonItem>
          ))}
        </IonList>
      </IonContent>
    </IonPage>
  );
};

export default Dashboard;
```

### Step 6: Routing Setup
`App.tsx`:

```tsx
import { Redirect, Route } from 'react-router-dom';
import { IonApp, IonRouterOutlet } from '@ionic/react';
import { IonReactRouter } from '@ionic/react-router';
import Login from './pages/Login';
import Dashboard from './pages/Dashboard';

const App: React.FC = () => {
  const isLoggedIn = !!localStorage.getItem('user');

  return (
    <IonApp>
      <IonReactRouter>
        <IonRouterOutlet>
          <Route exact path="/" component={Login} />
          <Route path="/dashboard" render={() =>
            isLoggedIn ? <Dashboard /> : <Redirect to="/" />} />
        </IonRouterOutlet>
      </IonReactRouter>
    </IonApp>
  );
};

export default App;
```

## Conclusion
This Ionic + React app shows how to create a modern mobile-style interface using core UI components. It includes authentication, header nav, popovers, cards, and dynamic task listing using IonList.
