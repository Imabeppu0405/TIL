# Contextについて

グローバルで値を管理する方法。Propsのバケツリレーを防ぐことができる。

```js
//UserProvider.jsx

import { createContext, useState } from "react";

//Contextの作成
export const UserContext = createContext({});

//Providerの作成
export const UserProvider = (props) => {
  const { children } = props;
  const [userName, setUserName] = useState("太郎");
  return (
    <UserContext.Provider value={{ userName, setUserName }}>
      {children}
    </UserContext.Provider>
  );
};

```

```js
//App.js

import { UserProvider } from "./UserProvider";
import { ContextText } from "./ContextText";
import { ChangeButton } from "./ChangeButton";

export default function App() {
  return (
    //Providerで囲むことでContextが使えるようになる
    <UserProvider>
      <ContextText />
      <ChangeButton />
    </UserProvider>
  );
}
```

```js
//ContextText.jsx

//import { useContext } from "react";
//import { UserContext } from "./UserProvider";
import { useUser } from "./UseUser";

export const ContextText = () => {
  //Contextに保存した値を扱う
  const { userName } = useUser();/*本来はuseContext(UserContext)*/
  return <p>{userName}</p>;
};
```

```js
//ChangeButton.jsx

//import { useContext } from "react";
//import { UserContext } from "./UserProvider";
import { useUser } from "./UseUser";

export const ChangeButton = () => {
  //Contextに保存する関数を扱う
  const { setUserName, userName } = useUser();/*本来はuseContext(UserContext)*/
  const onClick = () => {
    userName === "太郎" ? setUserName("二郎") : setUserName("太郎");
  };
  return <button onClick={onClick}>Change</button>;
};

```

```js
//UseUser.jsx

import { useContext } from "react";
import { UserContext } from "./UserProvider";

export const useUser = () => useContext(UserContext);
//これをすることでHooksっぽくContextを使える
```

Contextを使う際は、再レンダリングが多くなりがちなことに注意。

→これを防ぐためにuseMemoを使う。