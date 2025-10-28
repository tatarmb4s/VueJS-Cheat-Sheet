# React Code Examples

## Declare State

```jsx
import { useState } from "react";

export default function Name() {
  const [name] = useState("John");

  return <h1>Hello {name}</h1>;
}
```

## Update State

```jsx
import { useEffect, useState } from "react";

export default function Name() {
  const [name, setName] = useState("John");

  useEffect(() => {
    setName("Jane");
  }, []);

  return <h1>Hello {name}</h1>;
}
```

## Computed State

```jsx
import { useState } from "react";

export default function DoubleCount() {
  const [count] = useState(10);
  const doubleCount = count * 2;

  return <div>{doubleCount}</div>;
}
```

## Minimal Template

```jsx
import { useState } from "react";

export default function Name() {
  const [name] = useState("John");

  return <h1>Hello {name}</h1>;
}
```

## Styling

### CSS Import

```jsx
import "./style.css";

export default function CssStyle() {
  return (
    <>
      <h1 className="title">I am red</h1>
      <button style={{ fontSize: "10rem" }}>I am a button</button>
    </>
  );
}
```

### CSS File

```css
.title {
  color: red;
}
```

## Looping

```jsx
export default function Colors() {
  const colors = ["red", "green", "blue"];
  return (
    <ul>
      {colors.map((color) => (
        <li key={color}>{color}</li>
      ))}
    </ul>
  );
}
```

## Event Handling

```jsx
import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);

  function incrementCount() {
    setCount((count) => count + 1);
  }

  return (
    <>
      <p>Counter: {count}</p>
      <button onClick={incrementCount}>+1</button>
    </>
  );
}
```

## DOM Ref

```jsx
import { useEffect, useRef } from "react";

export default function InputFocused() {
  const inputElement = useRef(null);

  useEffect(() => inputElement.current.focus(), []);

  return <input type="text" ref={inputElement} />;
}
```

## Conditional Rendering

```jsx
import { ref, computed } from "react";

const TRAFFIC_LIGHTS = ["red", "orange", "green"];
const lightIndex = ref(0);

const light = computed(() => TRAFFIC_LIGHTS[lightIndex.value]);

function nextLight() {
  lightIndex.value = (lightIndex.value + 1) % TRAFFIC_LIGHTS.length;
}

// Template:
<button onClick={nextLight}>Next light</button>
<p>Light is: {light}</p>
<p>
  You must
  <span v-if={light === 'red'}>STOP</span>
  <span v-else-if={light === 'orange'}>SLOW DOWN</span>
  <span v-else-if={light === 'green'}>GO</span>
</p>
```

## Lifecycle: On Mount

```jsx
import { useState, useEffect } from "react";

export default function PageTitle() {
  const [pageTitle, setPageTitle] = useState("");

  useEffect(() => {
    setPageTitle(document.title);
  }, []);

  return <p>Page title: {pageTitle}</p>;
}
```

## Lifecycle: On Unmount

```jsx
import { useState, useEffect } from "react";

export default function Time() {
  const [time, setTime] = useState(new Date().toLocaleTimeString());

  useEffect(() => {
    const timer = setInterval(() => {
      setTime(new Date().toLocaleTimeString());
    }, 1000);

    return () => clearInterval(timer);
  }, []);

  return <p>Current time: {time}</p>;
}
```

## Props

### Parent Component (App.jsx)

```jsx
import UserProfile from "./UserProfile.jsx";

export default function App() {
  return (
    <UserProfile
      name="John"
      age={20}
      favouriteColors={["green", "blue", "red"]}
      isAvailable
    />
  );
}
```

### Child Component (UserProfile.jsx)

```jsx
import PropTypes from "prop-types";

export default function UserProfile({
  name = "",
  age = null,
  favouriteColors = [],
  isAvailable = false,
}) {
  return (
    <>
      <p>My name is {name}!</p>
      <p>My age is {age}!</p>
      <p>My favourite colors are {favouriteColors.join(", ")}!</p>
      <p>I am {isAvailable ? "available" : "not available"}!</p>
    </>
  );
}

UserProfile.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number.isRequired,
  favouriteColors: PropTypes.arrayOf(PropTypes.string).isRequired,
  isAvailable: PropTypes.bool.isRequired,
};
```

## Emit to Parent

### Parent Component (App.jsx)

```jsx
import { useState } from "react";
import AnswerButton from "./AnswerButton.jsx";

export default function App() {
  const [isHappy, setIsHappy] = useState(true);

  function onAnswerNo() {
    setIsHappy(false);
  }

  function onAnswerYes() {
    setIsHappy(true);
  }

  return (
    <>
      <p>Are you happy?</p>
      <AnswerButton onYes={onAnswerYes} onNo={onAnswerNo} />
      <p style={{ fontSize: 50 }}>{isHappy ? "😊" : "😢"}</p>
    </>
  );
}
```

### Child Component (AnswerButton.jsx)

```jsx
import PropTypes from "prop-types";

export default function AnswerButton({ onYes, onNo }) {
  return (
    <>
      <button onClick={onYes}>YES</button>
      <button onClick={onNo}>NO</button>
    </>
  );
}

AnswerButton.propTypes = {
  onYes: PropTypes.func,
  onNo: PropTypes.func,
};
```

## Slots

### Parent Component (App.jsx)

```jsx
import FunnyButton from "./FunnyButton.jsx";

export default function App() {
  return <FunnyButton>Click me!</FunnyButton>;
}
```

### Child Component (FunnyButton.jsx)

```jsx
import PropTypes from "prop-types";

export default function FunnyButton({ children }) {
  return (
    <button
      style={{
        background: "rgba(0, 0, 0, 0.4)",
        color: "#fff",
        padding: "10px 20px",
        fontSize: "30px",
        border: "2px solid #fff",
        margin: "8px",
        transform: "scale(0.9)",
        boxShadow: "4px 4px rgba(0, 0, 0, 0.4)",
        transition: "transform 0.2s cubic-bezier(0.34, 1.65, 0.88, 0.925) 0s",
        outline: "0",
      }}
    >
      {children}
    </button>
  );
}

FunnyButton.propTypes = {
  children: PropTypes.node,
};
```

## Context

### Creating Context (UserContext.jsx)

```jsx
import { createContext } from "react";

export const UserContext = createContext();
```

### Parent Component (App.jsx)

```jsx
import { useState } from "react";
import UserProfile from "./UserProfile";
import { UserContext } from "./UserContext";

export default function App() {
  // In a real app, you would fetch the user data from an API
  const [user, setUser] = useState({
    id: 1,
    username: "unicorn42",
    email: "unicorn42@example.com",
  });

  function updateUsername(newUsername) {
    setUser((userData) => ({ ...userData, username: newUsername }));
  }

  return (
    <>
      <h1>Welcome back, {user.username}</h1>
      <UserContext.Provider value={{ ...user, updateUsername }}>
        <UserProfile />
      </UserContext.Provider>
    </>
  );
}
```

### Child Component (UserProfile.jsx)

```jsx
import { useContext } from "react";
import { UserContext } from "./UserContext";

export default function UserProfile() {
  const { username, email, updateUsername } = useContext(UserContext);

  return (
    <div>
      <h2>My Profile</h2>
      <p>Username: {username}</p>
      <p>Email: {email}</p>
      <button onClick={() => updateUsername("Jane")}>
        Update username to Jane
      </button>
    </div>
  );
}
```

## Form Input

```jsx
import { useState } from "react";

export default function InputHello() {
  const [text, setText] = useState("Hello world");

  function handleChange(event) {
    setText(event.target.value);
  }

  return (
    <>
      <p>{text}</p>
      <input value={text} onChange={handleChange} />
    </>
  );
}
```
