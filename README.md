# In case of Firebase Duplication Error, please reload the page. 

# allergy-crud-description
ReactJs CRUD, with Firebase Realtime DB.

## install

```sh
npm install
```

# run on test-mode

```sh
npm start
```


## write user data


```js
  writeUserData = () => {
    Firebase.database()
      .ref("/")
      .set(this.state);
    console.log("DATA SAVED");
  };
```

### get user data

```js
  getUserData = () => {
    let ref = Firebase.database().ref("/");
    ref.on("value", snapshot => {
      const state = snapshot.val();
      this.setState(state);
    });
  };

```

### update user data

```js
  updateData = user => {
    this.refs.uid.value = user.uid;
    this.refs.name.value = user.name;
    this.refs.birth_date.value = user.birth_date;
  };
```

### delete user data

```js
  removeData = user => {
    const { users } = this.state;
    const newState = users.filter(data => {
      return data.uid !== user.uid;
    });
    this.setState({ user: newState });
  };
```

### handle-user-data-submit

```js
  handleSubmit = event => {
    event.preventDefault();
    let name = this.refs.name.value;
    let birth_date = this.refs.birth_date.value;
    let uid = this.refs.uid.value;

    if (uid && name && birth_date) {
      const { users } = this.state;
      const devIndex = users.findIndex(data => {
        return data.uid === uid;
      });
      users[devIndex].name = name;
      users[devIndex].birth_date = birth_date;
      this.setState({ users });
    } else if (name && birth_date) {
      const uid = new Date().getTime().toString();
      const { users } = this.state;
      users.push({ uid, name, birth_date });
      this.setState({ users });
    }

    this.refs.name.value = "";
    this.refs.birth_date.value = "";
    this.refs.uid.value = "";
  };
```
