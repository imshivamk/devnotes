## 1. Database and ODM

MongoDB

- NoSQL Document Database
- Stores data in JSON-like **documents** within **collections** (similar to tables).
- Schema is flexible but can be defined and enforced with **Mongoose**.
- Good for fast development, hierarchical/nested data.

Mongoose

- ORM Object Data Modeling (ODM) library (a NodeJS package)
- helps define schemas/models, validate data, and query MongoDB in an easy way.
- enforces schemas—blueprints that define the structure, data types, validation rules, and default values for documents

>backend/
>	models/
>		Note.js
>		User.js
>	...


## 2. Define a User Model with Mongoose 

>models/User.js

```js
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
name:{
	type:String,
	required:true,
	trim:true
},
email:{
	type:String,
	required:true,
	unique:true,
	lowercase:true
},
password:{
	type:String,
	required:true
},
createdAt:{
	type:Date,
	default: Date.now
}
});

const User = mongoose.model('User', userSchema);
module.exports = User;
```

- `type`: Defines data type.
- `required`: Field is mandatory.
- `unique`: Ensures no duplicates.
- `trim`, `lowercase`: String sanitization.
- Default timestamp for creation.

## 3. Implement CRUD Operations

```js
const User = require('../models/User');
```

```js
// CREATE a new user
exports.createUser = async (req, res, next) => {
  try {
    const { name, email, password } = req.body;

    // Basic validation: needed for production use specialized validation libraries (later phase)
    if (!name || !email || !password) {
      return res.status(400).json({ message: 'Name, email, and password are required' });

    // Check if user exists
    const existingUser = await User.findOne({ email });
    if (existingUser) {
      return res.status(409).json({ message: 'User already exists' });
    }
    const user = new User({ name, email, password });
    await user.save();
    res.status(201).json(user);
    
  } catch (err) {
    next(err);
  }
};
```

```js
// READ all users
exports.getUsers = async (req, res, next) => {
  try {
    const users = await User.find().select('-password');  // Exclude password from result
    res.json(users);
  } catch (err) {
    next(err);
  }
};
```

```js
// READ one user by ID
exports.getUserById = async (req, res, next) => {
  try {
    const user = await User.findById(req.params.id).select('-password');
    if (!user) return res.status(404).json({ message: 'User not found' });
    res.json(user);
  } catch (err) {
    next(err);
  }
};
```

```js
// UPDATE user by ID
exports.updateUser = async (req, res, next) => {
  try {
    const { name, email } = req.body;
    const updatedUser = await User.findByIdAndUpdate(
      req.params.id,
      { name, email },
      { new: true, runValidators: true }
    ).select('-password');
    if (!updatedUser) return res.status(404).json({ message: 'User not found' });
    res.json(updatedUser);
  } catch (err) {
    next(err);
  }
};
```

```js
// DELETE user by ID
exports.deleteUser = async (req, res, next) => {
  try {
    const deletedUser = await User.findByIdAndDelete(req.params.id);
    if (!deletedUser) return res.status(404).json({ message: 'User not found' });
    res.json({ message: 'User deleted' });
  } catch (err) {
    next(err);
  }
};
```

## 4. Define Routes and Integrate in server.js

>Routes/userRoutes.js

```js
const express = require("express");
const router = express.Router();
const userController = require("./controllers/userController.js")

router.get('/', userController.getUsers);
router.post('/', userController.createUser);


module.exports = router;
```

