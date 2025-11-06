# 📚 **CLASSWORK: REACT-REDUX CONTACTS & POSTS MANAGER**

This repository is a comprehensive educational project (**classwork**) that demonstrates the integration of core concepts of **React**, **Redux Toolkit (RTK)**, and **React Router DOM**. The main goal is to consolidate skills in managing synchronous state (contacts) and asynchronous logic (loading posts and users).

---
## 🛠️ **TECHNOLOGY STACK**

| Category | Technologies | Purpose |
| :--- | :--- | :--- |
| **Frontend** | React, JavaScript (ES6+) | UI Components. |
| **State Management** | Redux Toolkit (RTK), React-Redux | Centralized state management (Store, Slices, Thunks). |
| **Routing** | React Router DOM | Navigation between pages (`/`, `/contacts`, `/posts`). |
| **Forms and Validation** | Formik, Yup | Creating controlled forms and validation schemas. |
| **HTTP Requests** | Axios | Executing asynchronous requests to external APIs. |
| **Utilities** | `classnames`, `react-icons`, `uuid` | Dynamic styling, icons, generating unique IDs. |

---
## 🧱 **REDUX TOOLKIT ARCHITECTURE**

The global Redux Store is divided into three independent slices, defined in `src/store/index.js`:

```javascript
const store = configureStore({
  reducer: {
    contactsList: contactsReducer, // Synchronous logic (Contacts)
    postsList: postsReducer,       // Asynchronous logic (Posts)
    usersList: usersReducer,       // Asynchronous logic (Users)
  },
});


---
## **1. SYNCHRONOUS STATE: `contactSlice.js`**

This slice manages the list of contacts (`contacts`). It uses standard **RTK reducers** for instantaneous state changes:

* **`createContact`**: Adds a new contact to the list with a unique id.
* **`removeContact`**: Removes a contact by its id.
* **`toggleFavourite`**: Toggles the `isFavourite` status of a contact.

---
## **2. ASYNCHRONOUS STATE: `postsSlice.js` and `usersSlice.js`**

`createAsyncThunk` is used to work with the external API (`jsonplaceholder.typicode.com`). This allows for easy management of the request lifecycle (**pending**, **fulfilled**, **rejected**) using **`extraReducers`**.

### **Thunk Operations**

| Slice | Thunk Operation | Description |
| :--- | :--- | :--- |
| **`postsSlice`** | `getPostsThunk` | Loads the list of posts. Manages `isFetching` and `error` states. |
| **`usersSlice`** | `getUsersThunk` | Loads the list of users. |

### **Data Normalization (`usersSlice`)**

Upon successful loading of users (`getUsersThunk.fulfilled`), user data is stored in two formats:

* **Array (`users`)**: The original array of objects.
* **Normalized Object (`normalizedUsers`)**: An object where the key is the user's `id`, which enables **fast lookup** of the author by `userId` on the posts page.

---
## 📄 **COMPONENTS AND PAGES**

### **🧭 ROUTING**

The main `App.jsx` component sets up routing:

* `/`: Home page.
* `/contacts`: Page for managing contacts (**ContactsPage**).
* `/posts`: Page for displaying asynchronously loaded posts (**PostsPage**).

### **☎️ CONTACTS PAGE (`ContactsPage`)**

* **`ContactsForm`**: Uses **Formik** and **Yup** (`CONTACTS_VALIDATION_SCHEMA`) for validating "Full name" and "Phone number" fields (including RegExp checks). Form submission dispatches the `createContact` action.
* **`ContactsList`**: Displays the list of contacts, connected to the `contactsList` state.
* **`ContactsListItem`**: Component for a single contact with buttons for removal (`removeContact`) and toggling favorite status (`toggleFavourite`).

### **📰 POSTS PAGE (`PostsPage`)**

This demonstrates working with asynchronous data and combining it:

* **`useEffect`**: The `getPostsThunk()` and `getUsersThunk()` Thunk operations are called on the initial render.
* **State Handling**: Displays **Loading...** while `isFetching` is `true` or an error message (**Error!!!**).
* **Data Linking**: The component retrieves posts and normalized users. Each post is displayed along with the author's name, which is quickly retrieved from `normalizedUsers` using `post.userId`.

---
## 🚀 **HOW TO RUN THE PROJECT**

To run this Redux application locally, you will need **Node.js** and **npm** (or yarn/pnpm).

```bash
# 1. Clone the repository
git clone <YOUR_REPOSITORY_URL>

# 2. Navigate to the project folder
cd react-redux-contacts-classwork

# 3. Install dependencies
npm install

# 4. Start the project in development mode
npm run dev
