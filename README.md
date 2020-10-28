### In case of Firebase Duplication Error, please reload the page. 

### allergy-crud-description
ReactJs CRUD, with Firebase Realtime DB.

### install

```sh
npm install
```

### run on test-mode

```sh
npm start
```


### write user data


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

### page rendering component:
```js
  render() {
    const { users } = this.state;
    return (
      <React.Fragment>
        <div class="jumbotron jumbotron-fluid">
          <div class="container">
            <h1 class="display-4">allergy-crud</h1>

          </div>
        </div>


        <div className="container">
          <div className="row">
            <div className="col-xl-12">
              <h1>allergy-crud users:</h1>
            </div>
          </div>
          <div className="row">
            <div className="col-xl-12">
              {users.map(user => (
                <div
                  key={user.uid}
                  className="card float-left"
                  style={{ width: "18rem", marginRight: "1rem" }}
                >
                  <div className="card-body">
                    <h5 className="card-title">{user.name}</h5>
                    <p className="card-text">{user.birth_date}</p>
                    <button
                      onClick={() => this.removeData(user)}
                      className="btn btn-link"
                    >
                      Delete
                    </button>
                    <button
                      onClick={() => this.updateData(user)}
                      className="btn btn-link"
                    >
                      Edit
                    </button>
                  </div>
                </div>
              ))}
            </div>
          </div>
          <div className="row">
            <div className="col-md-6">
              <h1>Add User:</h1>
              <form onSubmit={this.handleSubmit}>
                <div className="form-group">
                  <input type="hidden" ref="uid" />
                  <div className="form-group">
                    <label>Name</label>
                    <input
                      type="text"
                      ref="name"
                      className="form-control"
                      placeholder="Name"
                    />
                  </div>

                  <div className="form-group">
                    <label>Birth Date</label>
                    <input
                      type="text"
                      ref="birth_date"
                      className="form-control"
                      placeholder="Birth Date"
                    />
                  </div>
                </div>

                <div class="form-group">
                  <label for="exampleFormControlSelect1">Gender</label>
                  <select class="form-control" id="exampleFormControlSelect1">
                    <option ref="male">Male</option>
                    <option ref="female">Female</option>
                    <option ref="non_binary">Non-Binary</option>
                  </select>
                </div>

                <div class="form-group">
                  <div class="col-sm-4">
                    <i class="fas fa-search h4 text-body" />
                  </div>

                  <div class="col-md-10">
                    <input class="form-control form-control-lg form-control-borderless" type="search" placeholder="Search allergies" />
                  </div>
                  <br />
                  <div class="col-md-10">
                    <button class="btn btn-sm btn-success" type="submit">Search</button>
                  </div>

                </div>


                <button type="submit" className="btn btn-primary">
                  Save
                </button>
              </form>
            </div>
          </div>

        </div>
      </React.Fragment>
    );
  }
}

```
